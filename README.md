# Piccolo Architecture Decision Records

This repository tracks the significant architectural decisions made for the [Piccolo OS project](https://github.com/AtDexters-Lab/piccolo-os). It serves as a public log that explains not just *what* we've decided, but *why* we've made those choices.

## Why We Maintain ADRs

Our core purpose at Piccolo is to empower users with digital self-sufficiency on an open platform. Fundamental to this mission is building a respected and trustworthy name in the open-source community , which we believe can only be achieved through radical transparency.

This repository is a key part of that commitment. By documenting our decisions in the open, we aim to:

* **Foster Transparency & Trust:** Provide a clear and public rationale for our architectural direction.
* **Engage the Community:** Create a forum for the community to understand, discuss, and contribute to the core design of Piccolo OS.
* **Create a Historical Record:** Preserve the context and consequences of our decisions for future contributors and our own team.

## What is an ADR?

An Architecture Decision Record (ADR) is a short document that captures a single, important architectural decision. Each ADR describes the context of the decision, the decision itself, and the consequences of that choice. They are stored as version-controlled markdown files.

## The Process

1.  Anyone can propose a new ADR by copying the `template.md` file, creating a new file named `NNN-short-title.md` (where NNN is the next available number), and submitting a Pull Request.
2.  The ADR will start in the **"Proposed"** state.
3.  Discussion on the proposed ADR will happen within the Pull Request.
4.  Once consensus is reached, the ADR will be merged and its status updated to **"Accepted"** or **"Rejected"**.
5.  An **"Accepted"** ADR represents the current, agreed-upon architecture for that specific area.

## Index of ADRs

| ID  | Title                               | Status   |
| --- | ----------------------------------- | -------- |
| 001 | [Custom Update Orchestration over Nebraska](./001-custom-update-orchestration.md) | Proposed |
