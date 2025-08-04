# 001: Custom Update Orchestration over Nebraska

* **Status:** Accepted
* **Author(s):** Abhishek Borar, Piccolo - Cofounder
* **Date:** 2025-08-04

## Context

Piccolo OS, being based on Flatcar Container Linux, requires a secure and reliable mechanism for delivering Over-the-Air (OTA) updates to all devices, including both official hardware and community-run nodes. Flatcar's standard update mechanism uses a client on the device (`update_engine`) that communicates with a Nebraska update server, which is an implementation of Google's Omaha protocol.

A core part of the Piccolo business model is an annual subscription that provides access to our managed network services, which includes hassle-free updates and access to the federated storage network. Therefore, our update mechanism must be able to check the subscription status of a device before delivering an update.

This presented two potential architectural paths:
1.  Use the standard Nebraska server and attempt to integrate our custom subscription and authentication logic into it.
2.  Build a custom, high-level orchestration mechanism that uses our central `piccolo-server` to manage update entitlement and delivery.

The decision was required to finalize the architecture for the core OS update flow, a critical component for both security and our business model.

## Decision

We will implement a custom update orchestration mechanism managed by our own software components.

The architecture will be as follows:
1.  The `piccolod` service running on each Piccolo device will be responsible for initiating update checks.
2.  It will periodically poll a purpose-built API endpoint on the central `piccolo-server`. This request will include the device's unique, TPM-based identity for authentication.
3.  The `piccolo-server` will house all business logic. It will validate the device's identity and check its subscription status before responding.
4.  If an update is available and the device is entitled to it, the server will respond with a secure, temporary URL to the latest signed OS image (`piccolo-os-update.raw.gz`).
5.  `piccolod` will download this image and then instruct the local `update_engine` utility to apply the update from the downloaded file, leveraging Flatcar's native A/B partition scheme for a safe, atomic update.

## Consequences

### Positive:

* **Seamless Business Logic Integration:** This approach allows for a clean, simple, and direct implementation of our subscription-based entitlement checks within our existing `piccolo-server` infrastructure.
* **Reduced Infrastructure Complexity:** We avoid the significant operational overhead of deploying, managing, securing, and scaling a separate Nebraska server instance.
* **Full Control & Flexibility:** We define the entire API and update protocol, giving us full control to add metadata or modify the process in the future without being constrained by the Omaha protocol.
* **Architectural Consistency:** This decision reinforces `piccolod` as the single agent responsible for OS provisioning. It uses the same core logic (`download-and-write-image`) for both initial user-driven installations and subsequent OTA updates, reducing code duplication and increasing maintainability.

### Negative:

* **No Out-of-the-Box Advanced Features:** We forfeit the sophisticated features that Nebraska provides by default, such as channel management (e.g., alpha, beta, stable) and phased percentage-based rollouts. If these features become necessary in the future, we will have to build and maintain that logic ourselves within `piccolo-server`.
* **"Reinventing the Wheel":** We are taking on the responsibility of building the high-level update orchestration logic that Nebraska already provides.

### Neutral:

* This decision requires new development work:
    * A new, authenticated API endpoint must be built on `piccolo-server` to handle update checks and entitlement.
    * `piccolod` must be enhanced with the client logic to poll this endpoint, download the image, and interface with the local `update_engine` utility.
