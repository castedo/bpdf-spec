---
title: "Baseprint Document Format"
abstract: |
    **DOCUMENT TYPE**: Living Technical Specification
---

## Background

Websites such as <https://lens.perm.pub> and <https://pilot.perm.pub>
use free open-source software, such as the Python package
[Epijats](https://gitlab.com/perm.pub/epijats),
to render Baseprint document snapshots into HTML pages and PDF files.

Scope
-----

This document is a specification of the Baseprint Document Format (BpDF) for interoperability
with reference free open-source software implementations. As of 2024, the only
reference implementation is the Python package [Epijats](https://gitlab.com/perm.pub/epijats).
Epijats is used by the authoring software
[Baseprinter](https://gitlab.com/perm.pub/baseprinter),
the Single-Page Application (SPA)
[BaseprintLens](https://gitlab.com/perm.pub/baseprintlens),
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

Inside a BpDF directory, there is a file named `article.xml` encoded in a subset of the
[JATS XML format](../jats.md).
This specification refers to this sub-format as *Baseprint JATS XML*.


### Baseprint JATS XML

Any external entities are ignored by Baseprint software
and must not be relied upon for interoperability.
Specifically, any official JATS DTD must not be required to parse a Baseprint JATS XML
file.


Formal Criteria
---------------

### Directory Encoding

**Criterion**:
The directory is encoded such that its computed hash interoperates with
[Git software](https://en.wikipedia.org/wiki/Git) as a Git tree hash.

**Criterion**:
The directory is encoded such that its computed hash interoperates with
the hash following a `swh:1:dir:` prefix in a
[SWHID (SoftWare Hash IDentifier)](https://www.swhid.org/).

**Criterion**:
The directory is encoded such that its computed hash interoperates with
[Git software](https://en.wikipedia.org/wiki/Git) as a Git tree hash and
the hash following a `swh:1:dir:` prefix in a
[SWHID (SoftWare Hash IDentifier)](https://www.swhid.org/).

**Criterion**:
There is only one file in the directory and its filename is `article.xml`.
This file is in the Baseprint JATS XML format described in this specification.

**Criterion**:

> TODO:
> which file flags/attributes are ignored or used by Hidos?



### XML basics

**Criterion**:
"Well-formed" per the [XML 1.0](https://www.w3.org/TR/REC-xml/) W3C recommendation.

**Criterion**:
No dependency on any external XML DTD.


#### `<article>` root element

> TBD


### `<front>` element tree

**Criterion**:
`<front>` has no attributes.

> TBD


### `<body>` element tree

**Criterion**:
`<body>` has no attributes.


> TBD

**Definition**:
Symbol `{P_LEVEL_TAG}` means any one of:
```
<code>
<disp-quote>
<list>
<p>
<preformat>
<table-wrap>
```


### `<back>` element tree

**Criterion**:
`<back>` has no attributes.

> TBD


### HTML-like content

**Definition**:
Depending on the where XML elements are located in the XML document tree,
elements are implied to be in zero, one, or more of the following *classes*:
```
@HYPOTEXT
@HYPERTEXT
@P_ELEMENT
@CITATION
```
by the criteria of this specification.

**Criterion**:
`<p>` elements have only child elements with `{P_CHILD_TAG}`.


**Criterion**:
Elements in class `@HYPOTEXT` only have child elements that are also in class
`@HYPOTEXT`.

**Definition**:
Symbol `{TYPO_TAG}` means any of the following tags:
```
<bold>
<italic>
<monospace>
<sub>
<sup>
```

**Criterion**:
Elements with `{TYPO_TAG}` have no attributes.

**Criterion**:
`{TYPO_TAG}` elements in class `@HYPERTEXT` only have child elements that are also in
class `@HYPERTEXT`.

**Definition**:
Symbol `{LINK_TAG}` means any tag of:
```
<ext-link>
<xref>
```

**Criterion**:
`{LINK_TAG}` elements only have child elements in class `@HYPOTEXT`.

**Criterion**:
Every `<ext-link>` has an `href=` attribute from the XML namespace
`http://www.w3.org/1999/xlink`.

**Criterion**:
`<ext-link>` attributes are either `href=` or (optional) `ext-link-type=`.

**Criterion**:
Every `<ext-link>` attribute `ext-link-type=` takes the value `"uri"` (if present).

**Definition**:
The symbol `{P_CHILD_TAG}` means any of:
```
{TYPO_TAG}
{LINK_TAG}
<code>
<def-list>
<disp-quote>
<list>
<preformat>
```


#### List elements

> TODO

#### Table elements

**Criterion**:
`<table-wrap>` elements contains a single `<table>` element.

**Criterion**:
`<table>` child elements are `<thead>` or `<tbody>` elements.

**Criterion**:
`<thead>` and `<tbody>` child elements are `<tr>` elements.

**Criterion**:
`<table-wrap>`, `<table>`, `<thead>`, `<tbody>`, and `<tr>` elements have no attributes.

**Criterion**:
`<table-wrap>`, `<table>`, `<thead>`, `<tbody>`, and `<tr>` elements only contain
white-space between child elements.

**Criterion**:
`<tr>` child elements are `<th>` or `<td>` elements.

**Criterion**:
`<th>` and `<td>` only have child elements with a `{P_CHILD_TAG}`.

**Criterion**:
`<th>` and `<td>` attributes are optional `align=` with values `left`, `center`, or `right`.



Interoperability Issues
-----------------------

* HTML `<table>` has `<thead>` before `<tbody>`
* HTML `<table>` probably (?) has exactly one `<tbody>` and at most one `<thead>`

