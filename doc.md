---
title: "Baseprint Document Format"
abstract: |
    **DOCUMENT TYPE**: Living Technical Specification
---

## Background

Websites like [https://pilot.perm.pub](https://pilot.perm.pub)
use free open-source software, such as the Python package
[Epijats](https://gitlab.com/perm.pub/epijats),
to render Baseprint document snapshots into HTML pages and PDF files.

Scope
-----

This document is a specification of the Baseprint Document Format (BpDF) for interoperability
with reference free open-source software implementations. As of 2024, the only
reference implementation is the Python package [Epijats](https://gitlab.com/perm.pub/epijats).
Epijats is used by the authoring software
[Baseprinter](https://gitlab.com/perm.pub/baseprinter)
and the website generation software
[BaseprintPress](https://gitlab.com/perm.pub/baseprintpress).

This specification does not define potential BpDF features that are not implemented in any software.
The online forum at <https://baseprints.singlesource.pub> is available for communication
about this living specification, its reference implementations, and other specifications
related to Baseprint document snapshots.


Informal Description
--------------------

### Directory Encoding

Technically, BpDF is not a file format
but rather a format for a directory-like data structure.
This structure is addressable as
a [Git](https://en.wikipedia.org/wiki/Git) tree and [SWHID](https://swhid.org)
directory.

When working with BpDF data,
it is often temporarily stored in a file system directory.
However, for public long-term storage, BpDF data is preserved in a SWHID addressable Git tree or
an equivalent "directory" object in the Software Heritage Archive.

Inside BpDF, there is a file named `article.xml` encoded in a subset of the
[JATS XML format](../jats.md).
This specification refers to this sub-format as *Baseprint JATS XML*.
