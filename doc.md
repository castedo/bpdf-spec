<!-- copybreak off -->

---
title: "Baseprint Document Format (BpDF)"
abstract: |
    **DOCUMENT TYPE**: Living Technical Specification

    The Baseprint Document Format
    is the digital encoding of a Baseprint document snapshot.
    These snapshots are immutable and
    are referenced using a [SoftWare Hash IDentifier (SWHID)](https://swhid.org).
    This format is designed for self-archived scientific
    and technical documents for long-term redistribution.
    The XML component of the format is a subset of the [JATS XML
    Article Authoring Tag Set](https://jats.nlm.nih.gov/articleauthoring/) and
    approximates XML found in [PubMed Central](https://pmc.ncbi.nlm.nih.gov/).
    After archiving,
    document snapshots are rendered into HTML pages and PDF files
    by independent websites using Baseprint-compatible software.
---

<!-- copybreak off -->

Feedback
--------

In addition to email,
feedback can also be communicated through the GitHub repository of the source files for this
edition at [github.com/castedo/bpdf-spec/](https://github.com/castedo/bpdf-spec/).
The online forum at <https://baseprints.singlesource.pub> is also available for
discussions related to Baseprint topics and specifications.


Interoperability
----------------

Websites such as <https://lens.perm.pub> and <https://pilot.perm.pub>
use free open-source software, like the Python package
[Epijats](https://gitlab.com/perm.pub/epijats),
to render Baseprint document snapshots into HTML pages and PDF files.

This specification is for interoperability
with reference software implementations. As of 2024, the only
reference implementation is the Python package [Epijats](https://gitlab.com/perm.pub/epijats).
For this edition of this specification,
version 2.0 of Epijats is the reference software.
Epijats is used by the authoring software
[Baseprinter](https://gitlab.com/perm.pub/baseprinter),
the Single-Page Application (SPA)
[BaseprintLens](https://gitlab.com/perm.pub/baseprintlens),
and the website generation software
[BaseprintPress](https://gitlab.com/perm.pub/baseprintpress).

<!-- copybreak off -->


Snapshots vs. Successions
-------------------------

This specification of the Baseprint Document Format (BpDF) is for
Baseprint document *snapshots* rather than Baseprint document *successions*.
Snapshots are archived and redistributed as part of a
*Baseprint document successions* whose digital encoding format is specified by the
separate specification,
 the [Document Succession Git Layout (DSGL)](https://perm.pub/VGajCjaNP1Ugz58Khn1JWOEdMZ8).
While snapshots are immutable, successions, in contrast, can be amended.
The SWHID for a snapshot is distinct from the
[Document Succession Identifier (DSI)](https://perm.pub/1wFGhvmv8XZfPx0O5Hya2e9AyXo)
used to identify a succession.

### Document Dates

Like
[JATS XML Article Authoring Tag Set](https://jats.nlm.nih.gov/articleauthoring/)
[@jats_authoring],
BpDF does not store a date for a document.
Instead, dates for a Baseprint document are recorded in the Git commit records of
the [Document Succession Git Layout (DSGL)](https://perm.pub/VGajCjaNP1Ugz58Khn1JWOEdMZ8)
[@dsgl].
These dates are set when an author amends a succession in DSGL with a snapshot as a new
edition.

<!-- copybreak off -->


JATS XML in a Directory
-----------------------

Technically, BpDF is not a file format
but rather a format for a directory-like data structure.
This structure is addressable as a
[SWHID](https://swhid.org) version 1 directory
(equivalent to a [Git](https://en.wikipedia.org/wiki/Git) tree).

When generating BpDF data, it is temporarily stored in a file system directory.
However, for long-term public storage, BpDF data is preserved in a
SWHID addressable directory in the [Software Heritage Archive](https://softwareheritage.org)
(or an equivalent tree in a Git repository).

At the top level of a BpDF directory, a file named `article.xml`
is encoded in a subset of the [JATS Article Authoring Tag
Set](https://jats.nlm.nih.gov/articleauthoring/) [@jats_authoring]
and is inspired and influenced by JATS4R [@jats4r_2015; @jats4r_2019].
Most of BpDF is a specification of this JATS XML file format,
which will be referred to as *Baseprint JATS XML*.

BpDF differs from the [Manuscript Exchange Common Approach (MECA)](https://meca.niso.org/)
in that a BpDF snapshot is automatically rendered into HTML pages and PDF files,
and is not designed for a non-automated publishing process.

<!-- copybreak off -->


Restyling to JATS4R
-------------------

Since the Baseprint JATS XML format is a small subset of JATS and is not intended for
real journal articles, a Baseprint JATS XML file can be restyled by adding fictitious data to
conform to the XML schema of a JATS4R validator or the [PMC Style
Checker](https://pmc.ncbi.nlm.nih.gov/tools/stylechecker/).

The [source code repository for Epijats](https://gitlab.com/perm.pub/epijats) includes
an XSLT file for such restyling. Some of the information that must be added is fictitious, such as
journal title. This restyling is for testing and facilitating possible
interoperability with other JATS systems.

<!-- copybreak off -->


Notable Features/Limitations
----------------------------

XML elements for tables, images and mathematics are absent from this edition of Baseprint JATS.
These important features of JATS are planned for a future edition.

Citations and references in Baseprint JATS XML
are styled by viewer
software that generate HTML pages and/or PDF files.
Authors do not control the citation styling.
References are in `<element-citation>` elements and not `<mixed-citation>`.
Furthermore,
there is no `publication-type` attribute.
Depending on the bibliographic fields present inside the `<element-citation>`,
viewer software must choose appropriate styling.
If the `<element-citation>` "looks" like a journal reference,
then viewer software should style it like a journal reference.
This means
it is challenging for viewer software to exactly match popular citation styles.
Software can approximate these styles
or rely on services like [Crossref](https://www.crossref.org/)
to get addition reference metadata
absent from the document snapshot data.

Another notable restriction
is `<xref ref-type="bibr">` XML elements
being inside top-level `<sup>` elements,
which are interpreted to have the semantic meaning
of a group of citations
to be styled together (e.g., `[7,11]`),
not necessarily superscripted text.

<!-- copybreak off -->


Formal Specification
--------------------

### Terminology

#### Element

In this specification, the term "element" means a specific XML element within an XML
document parse tree. The notation of `<foobar>` may refer to an XML tag or elements with
that tag, depending on context. When an element "has a tag" it never refers to a child element.

#### Criterion

The formal part of this specification is defined in terms of *criteria* and does not
prescribe what criteria XML files must or should satisfy.
Each formal criterion is a true or false statement for a given XML file.
Each criterion is documented to facilitate communication about which criteria
might not be satisfied in particular contexts.
Depending on the context, it might or might not make sense to satisfy specific criteria.
In general, the more criteria that are satisfied by an XML file, the higher
the level of interoperability it will achieve with the reference software of this specification.

<!-- copybreak off -->

#### Constraints

Due to the complexities of where XML elements can appear within an XML document tree,
some criteria are specified in terms of *constraints*.
XML elements in a document may have zero, one, or more constraints.
As an example, consider the following JATS paragraph:
```
<p>
    <sup>
        <xref rid="definition-e">
            e<sup>x</sup>
        </xref>
    </sup>
    <sup>
         <xref rid="r1" rid-type="bibr">1</xref>
    </sup>
</p>
```
The above example satisfies the criteria of this specification because
constraints can be assigned in the following manner:

* the first `<sup>` has constraint `$HYPERTEXT`,
* the second `<sup>` (nested inside the first) has constraint `$HYPOTEXT` (in addition
  to `$HYPERTEXT`), and
* the last `<sup>` has constraint `$CITATION`.

<!-- copybreak off -->


### Definitions/Symbols

**Definition**:
Criteria of this specification imply that XML elements in an XML document tree must have
zero, one, or more of the following *constraints*:
```
$CITATION
$HYPERTEXT
$HYPOTEXT
$P_CHILD
$P_LEVEL
```

**Definition**:
Symbol `{TYPO_TAG}` means any one of the following tags:
```
<bold>
<italic>
<monospace>
<sub>
<sup>
```


### Snapshot Directory Encoding

**Criterion**:
The directory is encoded such that its computed hash interoperates with
[Git software](https://en.wikipedia.org/wiki/Git) as a Git tree hash.

**Criterion**:
The directory is encoded such that its computed hash interoperates with
the hash following the `swh:1:dir:` prefix of a
[SWHID (SoftWare Hash IDentifier)](https://www.swhid.org/).

**Criterion**:
There is only one file in the directory and its filename is `article.xml`.
This file is in the Baseprint JATS XML format described in this specification.

**Criterion**:
The tree (directory) entry for `article.xml` has a normal file mode in Git and does not
have the executable bit set.

<!-- copybreak off -->


### XML Basics

**Criterion**:
"Well-formed" per the [XML 1.0](https://www.w3.org/TR/REC-xml/) W3C recommendation.

**Criterion**:
No dependency on any external XML DTD (not even a dependency to an official JATS DTD).

**Criterion**:
The XML prefix `ali` is used for any and all elements and attributes using the XML
namespace `http://www.niso.org/schemas/ali/1.0/` by relying on the declaration
```
xmlns:ali="http://www.niso.org/schemas/ali/1.0/"
```

**Criterion**:
The XML prefix `xlink` is used for any and all elements and attributes using the XML
namespace `http://www.w3.org/1999/xlink` by relying on the declaration
```
xmlns:xlink="http://www.w3.org/1999/xlink"
```

**Criterion**:
The following elements contain only optional whitespace between the start tag, any child
elements, and the end tag:

* `<article-meta>`
* `<article>`
* `<back>`
* `<contrib-group>`
* `<contrib>`
* `<date-in-citation>`
* `<disp-quote>`
* `<element-citation>`
* `<front>`
* `<license>`
* `<permissions>`
* `<person-group>`
* `<ref-list>`
* `<ref>`
* `<sec>`
* `<title-group>`


### Minimal attributes

**Criterion**:
The following elements have no attributes:

* `<abstract>`
* `<article-meta>`
* `<back>`
* `<body>`
* `<bold>`
* `<break>`
* `<code>`
* `<comment>`
* `<contrib-group>`
* `<copyright-statement>`
* `<day>`
* `<def-item>`
* `<def-list>`
* `<disp-quote>`
* `<element-citation>`
* `<elocation-id>`
* `<etal>`
* `<fpage>`
* `<front>`
* `<isbn>`
* `<issn>`
* `<issue>`
* `<italic>`
* `<license-p>`
* `<license>`
* `<list-item>`
* `<lpage>`
* `<monospace>`
* `<month>`
* `<name>`
* `<permissions>`
* `<preformat>`
* `<publisher-loc>`
* `<publisher-name>`
* `<ref-list>`
* `<source>`
* `<string-name>`
* `<sub>`
* `<suffix>`
* `<sup>`
* `<title-group>`
* `<uri>`
* `<volume>`
* `<year>`


**Criterion**:
The elements with the following tags only have the following possible attributes:

```
TAG                   POSSIBLE ATTRIBUTES
<article>             lang=
<contrib>             contrib-type=    id=
<date-in-citation>    content-type=
<ext-link>            ext-link-type=   href=          
<license_ref>         content-type=
<list>                list-type=
<person-group>        person-group-type=
<pub-id>              pub-id-type=
<sec>                 id=
```

<!-- copybreak off -->


### `<article>` element

**Criterion**:
`<article>` is the root element of the XML document.

**Criterion**:
The `<article>` attribute `lang=` (if present) has value `"en"`.

**Criterion**:
`<article>` has a sequence of child elements matching regular expression:
```
(<front>) (<body>) (<back>)?
```


### `<front>` element tree

**Criterion**:
`<front>` has exactly one child element `<article-meta>`.


**Criterion**:
`<article-meta>` has a sequence of child elements matching regular expression:
```
(<title-group>) (<contrib-group>) (<permissions>)? (<abstract>)
```

**Criterion**:
`<title-group>` has exactly one child element `<article-title>`.

**Criterion**:
`<article-title>` has constraint `$HYPERTEXT`.

**Criterion**:
`<contrib-group>` has only child elements with tag `<contrib>`.

**Criterion**:
`<contrib>` elements have an attribute of `contrib-type=` with value
`"author"`.

**Criterion**:
`<contrib>` elements have only child elements with a tag of any one of:

* `<name>` (exactly one)
* `<contrib-id>` (zero or one)
* `<email>` (zero or one)

**Criterion**:
`<name>` elements have only child elements with any one of the following tags:
```
<surname>
<given-names>
<suffix>
```
and at most one child element for each tag.

**Criterion**:
`<surname>`, `<given-names>`, and `<suffix>` have string content with no child elements.

**Criterion**:
`<contrib-id>` has exactly one attribute with value `contrib-id-type="orcid"`.

**Criterion**:
`<contrib-id>` has just string content of a valid ORCID including the
`https://orcid.org/` prefix.

**Criterion**:
`<permissions>` has only child elements `<copyright-statement>` and `<license>` (zero or
one each).

**Criterion**:
`<copyright-statement>` has constraint $HYPERTEXT.

**Criterion**:
`<license>` has only child elements `<license-p>` and `<license_ref>`.

**Criterion**:
`<license-p>` has constraint $HYPERTEXT.

**Criterion**:
`<license_ref>` tags are in the XML namespace:

`"http://www.niso.org/schemas/ali/1.0/"`.

**Criterion**:
`<license_ref>` element content is just a string (URL).

**Criterion**:
Attribute values of `content-type=` of element `<license_ref>` are any one of the
following:
```
"cc0license"
"ccbylicense"
"ccbysalicense"
"ccbynclicense"
"ccbyncsalicense"
"ccbyndlicense"
"ccbyncndlicense"
```

**Criterion**:
If the non-whitespace string contents of `<license_ref>` have one of the following prefixes:
```
"https://creativecommons.org/publicdomain/zero/"
"https://creativecommons.org/licenses/by/"
"https://creativecommons.org/licenses/by-sa/"
"https://creativecommons.org/licenses/by-nc/"
"https://creativecommons.org/licenses/by-nc-sa/"
"https://creativecommons.org/licenses/by-nd/"
"https://creativecommons.org/licenses/by-nc-nd/"
```
then the `content-type=` value, if present, must equal the corresponding respective
value:
```
"cc0license"
"ccbylicense"
"ccbysalicense"
"ccbynclicense"
"ccbyncsalicense"
"ccbyndlicense"
"ccbyncndlicense"
```

**Criterion**:
Element `<abstract>` contains a sequence of child elements with tags
matching the regular expression:

`(<p>)* (<sec>)*`

<!-- copybreak off -->


### `<body>` element tree

**Criterion**:
Element `<body>` contains a sequence of child elements with tags
matching the regular expression:

`($P_LEVEL)* (<sec>)*`

**Criterion**:
`<sec>` elements contain a sequence of child elements with tags and constraints matching
the regular expression:

`(<title>)? ($P_LEVEL)* (<sec>)*`

**Definition**:
Elements with constraint `$P_LEVEL` have any one of the following tags:
```
<code>
<disp-quote>
<list>
<p>
<preformat>
```

**Criterion**:
Child elements under `<title>` have any of the following tags:
```
<break>
<ext-link>
<xref>
{TYPO_TAG}
```

**Criterion**:
`<break>` elements are empty XML elements.

<!-- copybreak off -->


### `<back>` element tree

**Criterion**:
`<back>` has exactly one child element `<ref-list>`.

**Criterion**:
The `<ref-list>` element contain a sequence of child elements with tags matching the
regular expression:

`(<title>)? (<ref>)*`

**Criterion**:
`<ref>` elements have one attribute and it is `id=`.

**Criterion**:
`<ref>` elements have exactly one child element of `<element-citation>`.


**Criterion**:
`<element-citation>` elements only have child elements with any one of the following tags:
```
<article-title>
<comment>
<date-in-citation>
<day>
<edition>
<elocation-id>
<fpage>
<isbn>
<issn>
<issue>
<lpage>
<month>
<person-group>
<pub-id>
<publisher-loc>
<publisher-name>
<source>
<uri>
<volume>
<year>
```

**Criterion**:
For `<element-citation>` elements, there are one or zero child elements for each
possible tag, with the exception of `<pub-id>`, which can appear more than once.

**Criterion**:
For `<element-citation>` elements, all child elements with tag `<pub-id>` have different
values for attribute `pub-id-type=`.

**Criterion**:
`<person-group>` elements have an attribute `person-group-type=` with either value
`"author"` or `"editor"`.

**Criterion**:
`<person-group>` elements only have child elements with any of the following tags:
```
<name>
<string-name>
<etal>
```
**Criterion**:
`<string-name>` elements have string content only.

**Criterion**:
`<person-group>` elements have no more than one `<etal/>` child element.

**Criterion**:
`<etal>` elements are empty XML elements.

**Criterion**:
The following elements under `<element-citation>` have string content only:
```
<article-title>
<comment>
<elocation-id>
<fpage>
<isbn>
<issn>
<issue>
<lpage>
<publisher-loc>
<publisher-name>
<source>
<uri>
<volume>
```

**Criterion**:
`<year>`, `<month>`, and `<day>` elements have just an integer as content and do not have any non-digit
characters.

**Criterion**:
`<date-in-citation>` attribute `content-type=` equals value `"access-date"`.

**Criterion**:
Child elements `<year>`, `<month>`, and `<day>` appear at most once under their parent
element.

**Criterion**:
`<date-in-citation>` elements contain child element `<year>`.

**Criterion**:
`<date-in-citation>` has child element `<month>` only if `<year>` is also present.

**Criterion**:
`<date-in-citation>` has child element `<day>` only if `<month>` is also present.

**Criterion**:
`<date-in-citation>` elements do not contain any child elements other than `<year>`, `<month>`, or `<day>`.

**Criterion**:
`<edition>` elements have just an integer as content and do not have any non-digit
characters.

**Criterion**:
`<pub-id>` `pub-id-type=` attributes have values `"doi"` or `"pmid"`.

**Criterion**:
`<pub-id>` elements with attribute value `pub-id-type="doi"` have string content
that starts with "10." and not "http".

<!-- copybreak off -->


### HTML-like content

**Criterion**:
Elements with constraint `$HYPOTEXT` have tag `{TYPO_TAG}`.

**Criterion**:
Elements with constraint `$HYPOTEXT` have all child elements also with constraint
`$HYPOTEXT`.


#### Hypertext elements

**Criterion**:
Elements with constraint `$HYPERTEXT` have any one of the following tags:
```
<ext-link>
<xref>
{TYPO_TAG}
```

**Criterion**:
Elements with constraint `$HYPERTEXT` and `{TYPO_TAG}` have all child elements with
constraint `$HYPERTEXT`.

**Criterion**:
Elements with tag `<ext-link>` have all child elements with constraint `$HYPOTEXT`.

**Criterion**:
Every `<ext-link>` has an `href=` attribute from the XML namespace
`http://www.w3.org/1999/xlink`.

**Criterion**:
Every `<ext-link>` attribute `ext-link-type=` takes the value `"uri"` (if present).

**Criterion**:
Elements with tag `<xref>` have all child elements with constraint `$HYPOTEXT`.

**Criterion**:
Every element with tag `<xref>` and constraint `$HYPERTEXT` has an attribute `rid=`.

**Criterion**:
Every element with tag `<xref>` and constraint `$HYPERTEXT` has no attribute other than
`rid=`.


#### Paragraph elements

**Criterion**:
Elements with constraint `$P_CHILD` have one of the following tags:
```
<code>
<def-list>
<disp-quote>
<ext-link>
<list>
<preformat>
<xref>
{TYPO_TAG}
```

**Criterion**:
`<p>` elements have all child elements with constraint `$P_CHILD`.

**Criterion**:
Elements with constraint `$P_CHILD` and `{TYPO_TAG}` but not tag `<sup>` have constraint
`$HYPERTEXT`.

**Criterion**:
Elements with constraint `$P_CHILD` and tag `<sup>` have constraint `$HYPERTEXT` or `$CITATION`.


#### Citation elements

**Criterion**:
Elements with constraint `$CITATION` and tag `<sup>` have text content of:

* optional whitespace before the first child element
* optional whitespace after the last child element, and
* a comma and optional whitespace between child elements.

**Criterion**:
Elements with constraint `$CITATION` have all child elements with constraint `$CITATION` and tag `<xref>`.

**Criterion**:
Elements with constraint `$CITATION` and tag `<xref>` have an attribute of `ref-type=`
with value `bibr`.

**Criterion**:
Elements with constraint `$CITATION` and tag `<xref>` have exactly two attributes of
`rid=` and `ref-type=`.

**Criterion**:
Elements with constraint `$CITATION` and tag `<xref>` have a value for `rid=` that
matches the value of attribute `id=` in an element with tag `<ref>`.

**Criterion**:
Elements with constraint `$CITATION` and tag `<xref>` have content of only 
a single integer, surrounded by optional whitespace, and no child elements.
The integer corresponds to the position in `<ref-list>` of the `<ref>` element
with an `id=` attribute value that equals the `rid=` attribute of the `<xref>` element.


#### List elements

**Criterion**:
`<list>` element attributes `list-type=` have either value `"bullet"` or `"order"`.

**Criterion**:
`<list>` elements only have child elements with tag `<list-item>`.

**Criterion**:
`<list-item>` elements only have child elements with either tag `<p>` or `<list>`.

**Criterion**:
`<def-list>` elements only have child elements with tag `<def-item>`.

**Criterion**:
`<def-list>` elements only have child elements with either tag `<term>` or `<def>`.

**Criterion**:
`<term>` elements only have child elements with `{TYPO_TAG}` or `{LINK_TAG}`.

**Criterion**:
`<term>` elements have all child elements with constraint `$HYPERTEXT`.

**Criterion**:
`<def>` elements only have child elements with tag `<p>`.


#### Other elements

**Criterion**:
`<disp-quote>` elements only contain child elements with tag `<p>`.

**Criterion**:
`<code>` and `<preformat>` elements have all child elements with constraint `$HYPERTEXT`.



Interoperability Issues
-----------------------

* `<xref>` attribute `rid=` values that might not match an `id=` attribute
  of an element converted to HTML with the same `id=` (e.g., `<sec>`, `<ref>`).
