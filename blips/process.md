---
sidebar_position: 2
title: BLIP Process - Purpose and Guidelines
---

## Simple Summary

A process for reviewing and finalizing changes to Blit using Blit
Improvement Proposals (BLIPs).

A BLIP is a design document providing information to the Blit community, or describing a new feature or major change for Blit or its processes or environment.
The BLIP should provide a concise technical specification of the feature or change and a clear rationale.
The BLIP author is responsible for building consensus within the community and documenting dissenting opinions.

## Abstract

The BLIP process draws heavily from Ethereum's EIP process and Rust's RFC
process. It pertains primarily to the protocol and APIs of the Blit Hub
blockchain (Blit, for short), including Tendermint, Blit-SDK, IBC, and other modules.
Ideas for a change to the Blit protocols or APIs are first vetted in the
Blit forums and channels, and then formalized as a BLIP, detailing clearly the
proposed change. The BLIP includes all information necessary to implement the
change, including detailed specification and rationale. BLIPs are checked for
correctnes and process adherence by a select group of BLIP editors, though
finalization of a BLIP does not equate to acceptance into Blit. For that, BLIP
authors must turn to Blit Governance.

## Motivation

As the Blit ecosystem is very new and it is appropriate to start with good practices.

### BLIP Workflow

#### Shepherding a BLIP

Parties involved in the process are you, the champion or *BLIP author*, the *CODEOWNERS*, and the *Blit Core Developers*.

Before you begin writing a formal BLIP, you should vet your idea. Ask the Blit community first if an idea is original to avoid wasting time on something that will be rejected based on prior research. It is thus recommended to open a discussion thread on [the Blit discussion board](https://github.com/orgs/blitchain/discussions) to do this, but you can also use an [the Issues section of this repository].

Once the idea has been vetted, your next responsibility will be to present (by means of a BLIP) the idea to the reviewers and all interested parties, invite editors, developers, and the community to give feedback on the aforementioned channels. You should try and gauge whether the interest in your BLIP is commensurate with both the work involved in implementing it and how many parties will have to conform to it. For example, the work required for implementing a Core BLIP will be much greater than for an Interface and the BLIP will need sufficient interest from the Blit development teams. Negative community feedback will be taken into consideration and may prevent your BLIP from moving past the Draft stage.

#### BLIP Process

The following is the standardization process for all BLIPs in all tracks:

![BLIP Status Diagram](../website/static/img/BLIP-process.png)

**Idea** - An idea that is pre-draft. This is not tracked within the BLIP Repository.

**Draft** - The first formally tracked stage of a BLIP in development. A BLIP is merged by a BLIP Editor into the BLIP repository when properly formatted.

**Review** - A BLIP Author marks a BLIP as ready for and requesting Peer Review.

**Last Call** - This is the final review window for a BLIP before moving to `FINAL`. A BLIP editor will assign `Last Call` status and set a review end date (review-period-end), typically 14 days later.

If this period results in necessary normative changes it will revert the BLIP to `REVIEW`.

**Final** - This BLIP represents the final standard. A Final BLIP exists in a state of finality and should only be updated to correct errata and add non-normative clarifications.

**Stagnant** - Any BLIP in `DRAFT` or `REVIEW` if inactive for a period of 6 months or greater is moved to `STAGNANT`. A BLIP may be resurrected from this state by Authors or BLIP Editors through moving it back to `DRAFT`.

>*BLIP Authors are notified of any algorithmic change to the status of their BLIP*

**Withdrawn** - The BLIP Author(s) have withdrawn the proposed BLIP. This state has finality and can no longer be resurrected using this BLIP number. If the idea is pursued at later date it is considered a new proposal.

**Living** - A special status for BLIPs that are designed to be continually updated and not reach a state of finality. This includes most notably BLIP-1. Any changes to these BLIPs will move between `REVIEW` and `LIVING` states.

#### Submitting a BLIP

To create a BLIP:

- Fork this repository
- Copy `blip-template.md` to `BLIPS/blip-X.md` (don't assign a BLIP number yet)
- Fill in the BLIP template with a first draft of the BLIP
- Submit a pull request back to this repo. When formatted correctly, Draft BLIPs are given a BLIP number and merged to the
  repo by a BLIP editor.

At this point, the BLIP officially exists in Draft form with a designated BLIP
number. The process continues as follows:

- Submit additional pull requests to improve the Draft.
- Submit a pull request to change the BLIP Status from Draft to Review. At this
  point, the BLIP is officially ready for wider peer review from the Blit
  developers. They will open issues to further discuss elements of the BLIP, or submit PRs to make changes.
- Eventually, a BLIP editor will change the status to Last Call, setting a
  deadling for further review, following which they will change the status to
  Final.
- At this point, the BLIP is finalized and should not be changed other than for cosmetic purposes.
- The BLIP can now be submitted to the [Blit Governance] process for
  acceptance onto the network.

### What belongs in a successful BLIP?

Each BLIP should have the following parts:

- Preamble - RFC 822 style headers containing metadata about the BLIP, including the BLIP number, a short descriptive title (limited to a maximum of 44 characters), and the author details. See [below](#blip-header-preamble) for details.
- Simple Summary - simplified and layman-accessible explanation of the BLIP. Imagine an email subject line, GitHub PR title, or forum post title.
- Abstract - A short (~200 word) description of the technical issue being addressed.
- Motivation - A motivation section is critical for BLIPs that want to change the Blit protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the BLIP solves. BLIP submissions without sufficient motivation may be rejected outright.
- Documentation - Explain the proposal as if it was already deployed on the network and you were documenting it for another Blit user.
- Specification - The technical specification should describe the syntax and semantics of any new feature in detail. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Blit components.
- Drawbacks - Why should we not do this?
- Rationale - The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.
- Prior Art - Discuss prior art, both the good and the bad, in relation to this proposal
- Unresolved Questions - What will be resolved through the BLIP process, what
  will be resolved during implementation, and what related issues are out of
  scope?
- Backwards Compatibility - All BLIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The BLIP must explain how the author proposes to deal with these incompatibilities. BLIP submissions without a sufficient backwards compatibility treatise may be rejected outright.
- Test Cases - Test cases for an implementation are mandatory for BLIPs that are affecting consensus changes. Tests should either be inlined in the BLIP as data (such as input/expected output pairs, or included in `../assets/blip-###/<filename>`.
- Reference Implementation - An optional section that contains a reference/example implementation that people can use to assist in understanding or implementing this specification.
- Security Considerations - All BLIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life-cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. BLIP submissions missing the "Security Considerations" section will be rejected. A BLIP cannot proceed to status "Final" without a Security Considerations discussion deemed sufficient by the reviewers.
- Future Possibilities - Think about what the natural extension and evolution
  of your proposal would be and how it would affect the network and project as a
  whole in a holistic way.
- Copyright Waiver - All BLIPs must be in the public domain. See the bottom of this BLIP for an example copyright waiver.

### BLIP Formatting

BLIPs should be written in [markdown] format. There is a [template](https://github.com/blitchain/BLIPs/blob/main/blip-template.md) to follow.

#### BLIP Header Preamble

Each BLIP must begin with an [RFC 822](https://www.ietf.org/rfc/rfc822.txt) style header preamble, preceded and followed by three hyphens (`---`). The headers must appear in the following order. Headers marked with "*" are optional and are described below. All other headers are required.

`blip:` *BLIP number* (this is determined by the BLIP editor)

`title:` *BLIP title*

`author:` *a list of the author's or authors' name(s) and/or username(s), or name(s) and email(s). Details are below.*

`* discussions-to:` *a url pointing to the official discussion thread*

`status:` *Draft, Review, Last Call, Final, Stagnant, Withdrawn, Living*

`* review-period-end:` *date review period ends*

`type:` *Standards Track, Meta, or Informational*

`* category:` *Core or Interface* (fill out for Standards Track BLIPs only)

`created:` *date created on*

`* updated:` *comma separated list of dates*

`* requires:` *BLIP number(s)*

`* replaces:` *BLIP number(s)*

`* superseded-by:` *BLIP number(s)*

`* resolution:` *a url pointing to the resolution of this BLIP*

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

##### `author` header

The `author` header lists the names, email addresses or usernames of the authors/owners of the BLIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the `author` header value must be:

> Random J. User &lt;<address@dom.ain>&gt;

or

> Random J. User (@username)

if the email address or GitHub username is included, and

> Random J. User

if the email address is not given.

It is not possible to use both an email and a GitHub username at the same time. If important to include both, one could include their name twice, once with the GitHub username, and once with the email.

At least one author must use a GitHub username, in order to get notified on change requests and have the capability to approve or reject them.

##### `resolution` header

The `resolution` header is required for Standards Track BLIPs only. It contains a URL that should point to an email message or other web resource where the pronouncement about the BLIP is made.

##### `discussions-to` header

While a BLIP is a draft, a `discussions-to` header will indicate the mailing list or URL where the BLIP is being discussed. As mentioned above, examples for places to discuss your BLIP include an issue in this repo.

No `discussions-to` header is necessary if the BLIP is being discussed privately with the author.

As a single exception, `discussions-to` cannot point to GitHub pull requests.

##### `type` header

The `type` header specifies the type of BLIP: Standards Track, Meta, or Informational. If the track is Standards please include the subcategory (core, networking, or interface).

##### `category` header

The `category` header specifies the BLIP's category. This is required for standards-track BLIPs only.

##### `created` header

The `created` header records the date that the BLIP was assigned a number. Both headers should be in yyyy-mm-dd format, e.g. 2001-08-14.

##### `updated` header

The `updated` header records the date(s) when the BLIP was updated with "substantial" changes. This header is only valid for BLIPs of Draft and Active status.

##### `requires` header

BLIPs may have a `requires` header, indicating the BLIP numbers that this BLIP depends on.

##### `superseded-by` and `replaces` headers

BLIPs may also have a `superseded-by` header indicating that a BLIP has been rendered obsolete by a later document; the value is the number of the BLIP that replaces the current document. The newer BLIP must have a `replaces` header containing the number of the BLIP that it rendered obsolete.

#### Linking to other BLIPs

References to other BLIPs should follow the format `BLIP-N` where `N` is the BLIP number you are referring to.  Each BLIP that is referenced in a BLIP **MUST** be accompanied by a relative markdown link the first time it is referenced, and **MAY** be accompanied by a link on subsequent references.  The link **MUST** always be done via relative paths so that the links work in this GitHub repository, forks of this repository, the main BLIPs site, mirrors of the main BLIP site, etc.  For example, you would link to this BLIP with `[BLIP-1](./blip-1.md)`.

#### Auxiliary Files

Images, diagrams and auxiliary files should be included in a subdirectory of the `assets` folder for that BLIP as follows: `assets/blip-N` (where **N** is to be replaced with the BLIP number). When linking to an image in the BLIP, use relative links such as `../website/static/process/image.png`.

#### Style Guide

When referring to a BLIP by number, it should be written in the hyphenated form `BLIP-X` where `X` is the BLIP's assigned number.

### BLIP Ownership

It occasionally becomes necessary to transfer ownership of BLIPs to a new champion. In general, we'd like to retain the original author as a co-author of the transferred BLIP, but that's really up to the original author. A good reason to transfer ownership is because the original author no longer has the time or interest in updating it or following through with the BLIP process, or has fallen off the face of the 'net (i.e. is unreachable or isn't responding to email). A bad reason to transfer ownership is because you don't agree with the direction of the BLIP. We try to build consensus around a BLIP, but if that's not possible, you can always submit a competing BLIP.

If you are interested in assuming ownership of a BLIP, send a message asking to take over, addressed to both the original author and the BLIP editor. If the original author doesn't respond to the email in a timely manner, the BLIP editor will make a unilateral decision (it's not like such decisions can't be reversed :)).

### BLIP Editors

TODO - see [Unresolved Questions](#unresolved-questions).

The current BLIP editors are

- Ethan Buchman (@ebuchman)
- ...

#### BLIP Editor Responsibilities

For each new BLIP that comes in, an editor does the following:

- Read the BLIP to check if it is ready: sound and complete. The ideas must make technical sense, even if they don't seem likely to get to final status.
- The title should accurately describe the content.
- Check the BLIP for language (spelling, grammar, sentence structure, etc.), markup (GitHub flavored Markdown), code style

If the BLIP isn't ready, the editor will send it back to the author for revision, with specific instructions.

Once the BLIP is ready for the repository, the BLIP editor will:

- Assign a BLIP number (generally the PR number or, if preferred by the author, the Issue # if there was discussion in the Issues section of this repository about this BLIP)

- Merge the corresponding pull request

- Send a message back to the BLIP author with the next step.

Many BLIPs are written and maintained by developers with write access to the Blit codebase. The BLIP editors monitor BLIP changes, and correct any structure, grammar, spelling, or markup mistakes we see.

The editors don't pass judgment on BLIPs. We merely do the administrative & editorial part.

## Drawbacks

The BLIP process outlined here requires a number of existing repos and projects
to change their own processes and/or move them to this repository. There will be
new overhead to maintaining and governing this new process. There may also be
some redundancy as specifiction and design documents may be duplicated within
the BLIP repo and other repos. It's also one more repo to keep track off.

## Rationale

This is a well worn process in the Ethereum and Rust communities.
A single canonical place and standardized process across the Blit stack is
necessary to unite the efforts of the many Blit components and to provide
greater visibility into protocol and API changes to everyone.

## Prior Art

This document was derived heavily from [Ethereum's EIP-1], which was derived heavily from [Bitcoin's BIP-0001] written by Amir Taaki which in turn was derived from [Python's PEP-0001].
It is also inspired by the [Rust RFC process](https://github.com/rust-lang/rfcs), especially their
[RFC Template](https://github.com/rust-lang/rfcs/blob/master/0000-template.md) and
[rfc-0002](https://github.com/rust-lang/rfcs/blob/master/text/0002-rfc-process.md).
The bulk of the text was copied from [Ethereum's EIP-1] and modified as necessary.

Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger,
they are not responsible for its use in the Blit Improvement Process, and should not be bothered with technical questions specific to Blit or the BLIP.
Please direct all comments to the BLIP editors.

## Unresolved Questions

- Should we further decompose the "Core" category into perhaps Tendermint and
  State Machine, or even further, with distinctions like IBC, and more? Perhaps
  Tendermint and State Machine is the best place to start.
- What is the full scope of APIs that BLIPs should cover? Certainly any protocol
  breaking change to the blockchain or its state machine should have a BLIP, but
  what about API changes that don't involve a protocol breaking change or new
  protocol feature?
- Who are the initial editors and how are they chosen?
- How exactly will this BLIP process interact with other design and specification
  processes across Blit
- What do we use as our equivalent of Ethereum's AllCoreDevs designation and
  process ?
- This repo is a fork of the Ethereum EIP process and includes a Jekyll site,
  which is all pretty simple, but the Rust process and seems more advanced and
  there may be more worth adopting from their bots and build pipelines
- What license should we use? EIPs use CC0 for everything (ie. no license,
  copyright is just waived!), but Rust RFCs are licensed dual Apache/MIT,
  which means anything we copy from Rust needs to respect those licenses (and
  thus can't use CC0 - you can't waive someone else's copyright). Should we just license
  everything here Apache 2.0 then? This document contains virtually nothing
  copied verbatim from Rust RFC, but the current blip-template.md does.
