<!-- copybreak on -->

---
title: "Baseprint Document Format (BpDF)"
abstract: |
    **DOCUMENT TYPE**: Living Technical Specification

    The Baseprint Document Format
    is the digital encoding of a Baseprint document snapshot.
    These immutable snapshots
    are referenced using a [SoftWare Hash IDentifier (SWHID)](https://swhid.org).
    This format is designed for self-archived scientific
    and technical documents for long-term redistribution.
    BpDF contains an `article.xml` file that is HTML compatible and
    transformable to [JATS XML](https://jats.nlm.nih.gov/)[@jats].
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
version 2.2 of Epijats is the reference software.
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


JATS+XHTML XML File in a Directory
----------------------------------

Technically, BpDF is not a file format
but rather a format for a directory-like data structure.
This structure is addressable as a
[SWHID](https://swhid.org) version 1 directory
(equivalent to a [Git](https://en.wikipedia.org/wiki/Git) tree).

When generating BpDF data, it is temporarily stored in a file system directory.
However, for long-term public storage, BpDF data is preserved in a
SWHID addressable directory in the [Software Heritage Archive](https://softwareheritage.org)
(or an equivalent tree in a Git repository).

At the top level of a BpDF directory, a *Baseprint XML* file named `article.xml`
is encoded in a subset of [JATS XML](https://jats.nlm.nih.gov/)[@jats_authoring]
and XHTML.
Baseprint XML is inspired and influenced by JATS4R [@jats4r_2015; @jats4r_2019].
Most of BpDF is a specification of this XML file format,
which is referred to as *Baseprint XML*.

BpDF differs from the [Manuscript Exchange Common Approach (MECA)](https://meca.niso.org/)
in that a BpDF snapshot is automatically rendered into HTML pages and PDF files,
and is not designed for a non-automated publishing process.

<!-- copybreak off -->


### JATS-Transformable

A Baseprint XML file can be transformed into a JATS XML file that conforms
to the XML schema of a JATS4R validator or the [PMC Style
Checker](https://pmc.ncbi.nlm.nih.gov/tools/stylechecker/).
The [source code repository for Epijats](https://gitlab.com/perm.pub/epijats) includes
an XSL transformation file and script.
Some of the information that must be added is fictitious, such as
the journal title.

<!-- copybreak off -->


Notable Features/Limitations
----------------------------

### Tables, math, images, and footnotes

XML elements for tables, math, images, and footnotes are absent from this edition of BpDF.
These important features are planned for a future edition.

### Citation style

Citations and references in Baseprint XML
are styled by viewer
software that generates HTML pages and/or PDF files.
Authors do not control the citation styling.
References are in JATS `<element-citation>` elements and not JATS `<mixed-citation>`.
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
to get additional reference metadata
absent from the document snapshot data.

### Citation tuple elements

`<sup>` elements containing `<xref ref-type="bibr">` elements
are interpreted to have the semantic meaning
of a group of citations
to be styled together (e.g., `[7,11]`),
and not necessarily superscripted text.

<!-- copybreak off -->

### External Metadata

Metadata in a typical JATS XML document is sourced from both authors and journal
publishers. In this most common scenario, JATS XML serves as a vehicle for multiple
sources of metadata.
Baseprint XML differs from typical JATS XML documents in two ways:

1. a Baseprint document is designed for self-archiving/self-publishing by authors, and
2. Baseprint XML is contained within an *immutable* document *snapshot*.

With respect to a document snapshot, some metadata is *internal*, to be included in
Baseprint XML, while other metadata is *external* and intentionally not included.
For example, the JATS element 
`/article/front/article-meta/title-group/article-title` is internal metadata that is
sourced from an author and thus appropriately included in Baseprint XML.
In contrast, the JATS element
`/article/front/article-meta/history` is external metadata, and does not make sense
to store inside an immutable Baseprint document snapshot.

<!-- copybreak off -->


Formal Specification
--------------------

### Terminology

#### Criterion

The formal part of this specification is defined in terms of *criteria* and does not
prescribe what criteria XML files must or should satisfy.
Each formal criterion is a true or false statement for a given XML file.
Each criterion is documented to facilitate communication about which criteria
might not be satisfied in particular contexts.
Depending on the context, it might or might not make sense to satisfy specific criteria.
In general, the more criteria that are satisfied by an XML file, the higher
the level of interoperability it will achieve with the reference software of this specification.

#### Element

In the definition of a criterion, the term "element" refers to an XML element within an XML
document's parse tree. The notation `<foobar>` may refer to an XML tag or elements with
that tag, depending on the context. When an element "has a tag" it never refers to a child element.

<!-- copybreak on -->

#### Content

The contents of Baseprint XML elements fall into five categories:

mixed content
: both text and child elements

element-only content
: only child elements

text-only content
: text with no child elements

whitespace-only content
: non-empty whitespace content between a start and end tags (e.g., `<etal> </etal>`)

HTML void element[@void_element; @html] pseudo-content
: the absence of content implied by an element serialized in the XML self-closing tag
  syntax (e.g., `<br/>`)

Whitespace is in the narrow sense of the ASCII characters tab (9), linefeed
(10), vertical tab (11), formfeed (12), carriage return (13), and space (32).

<!-- copybreak off -->

#### Element varieties

Some XML elements with the same tag have differing semantics depending on their location
within an XML document tree.
For this reason, some criteria in this specification are specified in terms of *element varieties*.
Specifically, the elements
`<a>`, `<article-title>`, `<b>`, `<i>`, `<tt>`, `<sub>`, and `<sup>`
have multiple *varieties*.
For XML documents that satisfy the criteria of this specification, these elements will
unambiguously belong to exactly one of their varieties.

Elements with the following tags may be of the following varieties
based on the criteria of this specification.

```
ELEMENT TAG      ELEMENT VARIETIES
-----------      -----------------
<a>              ~OUT    ~IN
<article-title>  ~SELF   ~REF
<b>              ~HYPER  ~COPY  ~HYPO  ~MINI
<i>              ~HYPER  ~COPY  ~HYPO  ~MINI
<tt>             ~HYPER  ~COPY  ~HYPO
<sub>            ~HYPER  ~COPY  ~HYPO  ~MINI
<sup>            ~HYPER  ~COPY  ~HYPO  ~MINI  ~CITE
<section>        ~2  ~3  ~4  ~5  ~6
```

The notation `<foo>~BAR` is used to denote a `<foo>` element of the `~BAR` variety.
The notation of `<foo>` without any variety means a `<foo>` element of *any* variety.

Element varieties could be made unnecessary by using different XML tag names,
but they would be non-standard tag names not found in HTML or JATS XML.

As an example, consider the `<sup>` elements of the following paragraph:
```
<p>
    <sup>
        <a href="#definition-e">
            e<sup>x</sup>
        </a>
    </sup>
    <sup>
         <xref rid="r1" rid-type="bibr">1</xref>
    </sup>
</p>
```

Based on the criteria of this specification, the `<sup>` elements are of the following varieties:

* the first `<sup>` is of the variety `<sup>~HYPER`,
* the second `<sup>` (nested inside the first) is of the variety `<sup>~HYPO`, and
* the last `<sup>` is of the variety `<sup>~CITE`.

<!-- copybreak off -->


### Snapshot Directory Encoding

**Criterion #14435**:
The directory is encoded such that its computed hash interoperates with
[Git software](https://en.wikipedia.org/wiki/Git) as a Git tree hash.

**Criterion #16289**:
The directory is encoded such that its computed hash interoperates with
the hash following the `swh:1:dir:` prefix of a
[SWHID (SoftWare Hash IDentifier)](https://www.swhid.org/).

**Criterion #12743**:
There is only one file in the directory and its filename is `article.xml`.
This file is in the Baseprint XML format described in this specification.

**Criterion #14763**:
The directory (Git tree) entry for `article.xml` has a normal file mode in Git and does not
have the executable bit set.

<!-- copybreak on -->


### XML/HTML/CSS interoperability

**Criterion #15719**:
The file `article.xml` is "well-formed" per the [XML 1.0](https://www.w3.org/TR/REC-xml/) W3C recommendation.

**Criterion #13799**:
There is no parsing dependency on any external XML DTD.

**Criterion #13652**:
The only XML/HTML/SGML entities used are SGML numeric character references and the five
predefined entities of XML.

*Note*: Use of convenient HTML entities such as `&nbsp;` and `&copy;` do not satisfy
criterion #13652.

**Criterion #14199**:
No XML namespaces are necessary to parse `article.xml`.

*Comment:* XML namespace prefixes are not compatible with CSS.
JATS elements using XML namespace prefixes are avoided in
Baseprint XML by using HTML tags rather than JATS tags for HTML-like elements and tag name
`<license-ref>` instead of `<ali:license_ref>`.

**Criterion #18620**:
HTML void elements[@void_element; @html] (e.g., `<br/>`, `<col />`, ...) are serialized
with the self-closing XML syntax and not a pair of adjacent start and end tags (e.g.,
`<br/>` and not `<br></br>`).

**Criterion #15105**:
No elements other than HTML void elements[@void_element; @html] use the self-closing XML
syntax (e.g., `<br/>`).

**Criterion #11095**:
No XML elements are serialized as a pair of adjacent start and end tags (e.g.,
`<etal></etal>`). All XML elements that are not HTML void elements contain
some whitespace content (e.g. `<etal> </etal>`) or some text or some child elements.

*Comment:* The equivalency in XML of self-closing tag syntax (e.g., `<br/>`) and empty
content between start/close tags syntax (e.g., `<br></br>`) is incompatible with HTML
parsers. HTML parsers ignore the `/>` syntax and only rely on tag name to determine when
to expect a close tag. Some XML parsers do not support distinguishing between these two
XML syntaxes. Avoiding empty content between start/close tags avoids these parsing
incompatibilities.


**Criterion #10825**:
The element tree structure and contents of `article.xml` are preserved when parsed as
type `text/html` by the `DOMParser` object of the Web API of widely available web browsers.

*Note:* Criterion #10825 above is implied by the rest of the criteria of this specification.

<!-- copybreak on -->


### HTML content

#### Minimally formatted text

**Definition**:
Elements of variety `~MINI` have tag `<b>`, `<i>`, `<sub>`, or `<sup>`.

**Definition**:
The element set `{MINITEXT}` consists of the elements:
```
<b>~MINI
<i>~MINI
<sub>~MINI
<sup>~MINI
```

**Criterion #18662**:
Elements of variety `~MINI` contain mixed content with all child elements from
the set `{MINITEXT}`.

*Comment*: {MINITEXT} is used by `<article-title>~SELF`.

#### Copyable hypertext

Copyable hypertext is used by `<copyright-statement>` and `<license-p>`.
There are no citations, internal links, or cross references,
which would not make sense if copied out of the document.

**Definition**:
Elements of variety `~COPY` have tag `<b>`, `<i>`, `<tt>`, `<sub>`, or `<sup>`.

**Definition**:
The element set `{COPYTEXT}` consists of the elements:
```
<a>~OUT
<b>~COPY
<i>~COPY
<tt>~COPY
<sub>~COPY
<sup>~COPY
```

**Criterion #11694**:
Elements of variety `~COPY` contain mixed content with all child elements from the set
`{COPYTEXT}`.

*Comment*: The `{COPYTEXT}` element set is motivated by interoperability with Crossref,
which uses the [JATS publishing schema for `<copyright-statement>`](
https://jats.nlm.nih.gov/publishing/tag-library/1.4/element/copyright-statement.html).

#### Hypertext

**Definition**:
The element set `{HYPERTEXT}` consists of the elements:
```
<a>
<b>
<i>
<tt>
<sub>
<sup>
```

**Criterion #13724**:
The following elements contain mixed content with all child elements from the set
`{HYPERTEXT}`:
```
<b>~HYPER
<i>~HYPER
<tt>~HYPER
<sub>~HYPER
<sup>~HYPER
```

**Criterion #19901**:
The following elements do not have any attributes:
```
<b>
<i>
<tt>
<sub>
<sup>
```

#### Hypotext

**Definition**:
The element set `{HYPOTEXT}` consists of the elements:
```
<b>~HYPO
<i>~HYPO
<tt>~HYPO
<sub>~HYPO
<sup>~HYPO
```

**Criterion #10387**:
Elements from the set `{HYPOTEXT}` contain mixed content with all child elements from the set
`{HYPOTEXT}`.

<!-- copybreak on -->


#### Other elements

##### \<a>

**Criterion #19871**:
`<a>` contains mixed content with all child elements from the set `{HYPOTEXT}`.

**Criterion #10107**:
`<a>` elements are of variety `<a>~OUT` or `<a>~IN`.

**Criterion #17248**:
`<a>~IN` elements have only attribute `href=` and it equals a '#' (hash symbol)
followed by the value of an `id=` attribute of another element.

**Criterion #11997**:
`<a>~OUT` elements have only attribute `rel="external"` and attribute `href=` equal to an
`https:` or `http:` URL.

The [HTML Living Standard](https://html.spec.whatwg.org/multipage/links.html#link-type-external)[@html]
and the [Internet Assigned Numbers
Authority](https://www.iana.org/assignments/link-relations/link-relations.xhtml)
specify `rel="external"`.

##### \<br/>

**Criterion #18396**:
`<br/>` elements have no attributes and contain empty content.

##### \<code>

**Criterion #13634**:
`<code>` elements have no attributes.

**Criterion #15943**:
`<code>` elements contain mixed content with child elements from the set `{HYPERTEXT}`.

##### \<p>

The `<p>` element in this edition 2 is equivalent to the `<p>~HTML` element variety of
edition 1. The element variety distinction is no longer needed because all `<p>~WRAPPER`
related criteria have been dropped.

**Criterion #13912**:
`<p>` elements have no attributes.

**Criterion #14762**:
`<p>` elements contain mixed content with all child elements from the set `{HYPERTEXT}`.


##### \<pre>

**Criterion #10062**:
`<pre>` elements have no attributes.

**Criterion #18825**:
`<pre>` elements contain mixed content with child elements from the set `{HYPERTEXT}`.

<!-- copybreak off -->


#### List elements

##### \<ol> & \<ul>

**Criterion #13698**:
`<ol>` and `<ul>` elements have no attributes.

**Criterion #17842**:
`<ol>` and `<ul>` elements contain element-only content with only `<li>` child elements.

##### \<li>

**Criterion #18401**:
`<li>` elements have no attributes.

**Criterion #13486**:
`<li>` elements contain element-only content with child elements from the set `{P_LEVEL}`.

##### \<dl>

**Criterion #16653**:
`<dl>` elements have no attributes.

**Criterion #19568**:
`<dl>` elements contain element-only content with only `<div>` child elements.

##### \<div> child element of \<dl>

**Criterion #13056**:
`<div>` child elements of `<dl>` have no attributes.

**Criterion #11744**:
`<div>` child elements of `<dl>` contain element-only content with child elements of either `<dt>` or `<dd>`.

##### \<dt>

**Criterion #15106**:
`<dt>` elements have no attributes.

**Criterion #17876**:
`<dt>` elements contain mixed content with child elements from the set `{HYPERTEXT}`.

##### \<dd>

**Criterion #18382**:
`<dd>` elements have no attributes.

**Criterion #13562**:
`<dd>` elements contain element-only content with child elements from the set `{P_LEVEL}`.

<!-- copybreak off -->


### High-level structural elements

#### Highest-level elements

##### \<article>

**Criterion #15199**:
`<article>` is the root element of the XML document.

**Criterion #10864**:
`<article>` has no attributes.

**Criterion #16641**:
`<article>` contains element-only content with at most one child element per tag from the set:
```
<front>
<article-body>
<back>
```

##### \<front>

**Criterion #14001**:
`<front>` has no attributes.

**Criterion #12640**:
`<front>` contains element-only content with at most one child element of `<article-meta>`.

##### \<article-meta> element tree

**Criterion #13284**:
`<article-meta>` has no attributes.

**Criterion #11553**:
`<article-meta>` contains element-only content with at most one child element per tag from the set:
```
<title-group>
<contrib-group>
<permissions>
<abstract>
```

##### \<back> element tree

**Criterion #11019**:
`<back>` has no attributes.
 
**Criterion #18947**:
`<back>` contains element-only content with exactly one child element `<ref-list>`.


#### Content elements

**Definition**:
`{P_LEVEL}` denotes the set of elements:
```
<code>
<blockquote>
<dl>
<ol>
<p>
<pre>
<ul>
```

##### \<blockquote>

**Criterion #13925**:
`<blockquote>` elements have no attributes.

**Criterion #13249**:
`<blockquote>` elements contain element-only content with only `<p>` child elements.

##### \<abstract>

**Criterion #14631**:
`<abstract>` has no attributes.

**Criterion #17433**:
`<abstract>` contains element-only content with all child elements from the set
`{P_LEVEL}`.

##### \<article-body>

**Criterion #19029**:
`<article-body>` has no attributes.

**Criterion #11247**:
`<article-body>` contains element-only content with a sequence of child elements matching the regular
expression:

`({P_LEVEL})* (<section>~2)*`

##### \<section>

**Criterion #12167**:
`<section>` elements have no attributes or an `id=` attribute.

**Criterion #14586**:
`<section>~N` elements contain element-only content with a sequence of child elements matching the
regular expression:

`(<hN>)? ({P_LEVEL})* (<section>~(N+1))*`

for `N` equal to 2, 3, 4, or 5.

**Criterion #18843**:
`<section>~6` elements contain element-only content with a sequence of child elements matching the
regular expression:

`(<h6>)? ({P_LEVEL})* (<section>~6)*`

##### \<h2>, \<h3>, \<h4>, \<h5>, \<h6>

**Criterion #10699**:
`<h2>`, `<h3>`, `<h4>`, `<h5>`, and `<h6>` elements have no attributes.

**Criterion #14064**:
`<h2>`, `<h3>`, `<h4>`, `<h5>`, and `<h6>` elements contain mixed content with each child
element either `<br/>` or of the element set `{HYPERTEXT}`.

<!-- copybreak off -->


### Metadata elements

#### Article title

##### \<title-group>

**Criterion #15574**:
`<title-group>` has no attributes.

**Criterion #19365**:
`<title-group>` contains element-only content with at most one child element
`<article-title>~SELF`.

##### \<article-title>

**Criterion #17019**:
`<article-title>` elements have no attributes.

**Criterion #10037**:
`<article-title>` elements are either `<article-title>~SELF` or `<article-title>~REF`.

**Criterion #11294**:
`<article-title>~SELF` contains mixed content with child elements from the set
`{MINITEXT}`.

#### Contributors

##### \<contrib-group>

**Criterion #10923**:
`<contrib-group>` has no attributes.

**Criterion #17698**:
`<contrib-group>` contains element-only content with only child elements of `<contrib>`.

##### \<contrib>

**Criterion #17181**:
`<contrib>` elements have exactly one attribute of `contrib-type=` with value
`"author"`.

**Criterion #19818**:
`<contrib>` elements contain element-only content with child elements:

* `<name>` (exactly one)
* `<contrib-id>` (zero or one)
* `<email>` (zero or one)

##### \<name>

**Criterion #15691**:
`<name>` has no attributes.

**Criterion #12424**:
`<name>` contains element-only content with child elements from the set:
```
<surname>
<given-names>
<suffix>
```
and at most one child element for each tag.

##### \<surname>, \<given-names>, and \<suffix>

**Criterion #17569**:
`<surname>`, `<given-names>`, and `<suffix>` have no attributes.

**Criterion #17289**:
`<surname>`, `<given-names>`, and `<suffix>` contain text-only content.

##### \<contrib-id>

**Criterion #13828**:
`<contrib-id>` has exactly one attribute with value `contrib-id-type="orcid"`.

**Criterion #12150**:
`<contrib-id>` contains text-only content of a valid ORCID including the
`https://orcid.org/` prefix.


#### Permissions and licensing

##### \<permissions>

**Criterion #19885**:
`<permissions>` has no attributes.

**Criterion #11010**:
`<permissions>` contains element-only content of only child elements `<copyright-statement>` and
`<license>` (zero or one of each).

##### \<copyright-statement>

**Criterion #13932**:
`<copyright-statement>` has no attributes.

**Criterion #17441**:
`<copyright-statement>` contains mixed content with child elements of set `{COPYTEXT}`.

##### \<license>

**Criterion #19618**:
`<license>` has no attributes.

**Criterion #13667**:
`<license>` contains element-only content with child elements from the set:
```
<license-p>
<license_ref>
<ali:license_ref>
```

**Criterion #15516**:
`<license>` does not contains multiple child elements with the same tag.

**Criterion #16066**:
`<license>` does not contains both a `<license_ref>` child element and a
`<ali:license_ref>` child element.


##### \<license-p>

**Criterion #10671**:
`<license-p>` has no attributes.

**Criterion #10974**:
`<license-p>` contains mixed content with child elements of set `{COPYTEXT}`.

##### \<ali:license\_ref>

**Criterion #16170**:
`<ali:license_ref>` contains text-only content of a URL.

**Criterion #16811**:
`<ali:license_ref>` has no attribute or an attribute of `content-type=` with any one of
the following values:
```
"cc0license"
"ccbylicense"
"ccbysalicense"
"ccbynclicense"
"ccbyncsalicense"
"ccbyndlicense"
"ccbyncndlicense"
```

**Criterion #11510**:
If the non-whitespace text contents of `<ali:license_ref>` have one of the following prefixes:
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

<!-- copybreak off -->


### Bibliographic elements

#### Citation

##### \<xref>

**Criterion #14740**:
`<xref>` elements have exactly two attributes: `rid=` and `ref-type=`.

**Criterion #11027**:
`<xref>` elements have an attribute of `ref-type=` with the value `bibr`.

**Criterion #12086**:
`<xref>` elements have a value for `rid=` that
matches the value of the attribute `id=` of a `<ref>` element.

**Criterion #10484**:
`<xref>` elements contain text-only content of
a single integer (surrounded by optional whitespace).
The integer corresponds to the ordered position in `<ref-list>` of the `<ref>` element
with an `id=` attribute value matching the `rid=` attribute value of the `<xref>` element.

##### \<sup>~CITE

**Criterion #14278**:
`<sup>~CITE` elements only have child elements of `<xref>`.

**Criterion #12352**:
`<sup>~CITE` elements have mixed content with text-only content of:

* optional whitespace before the first child element,
* optional whitespace after the last child element, and
* a comma and optional whitespace between child elements.


#### Bibliography

##### \<ref-list>

**Criterion #14165**:
`<ref-list>` has no attributes.

**Criterion #12136**:
`<ref-list>` contains element-only content with only `<ref>` child elements.

##### \<ref>

**Criterion #18652**:
`<ref>` elements have one attribute, and it is `id=`.

**Criterion #15949**:
`<ref>` contains element-only content with exactly one child element of `<element-citation>`.

##### \<element-citation>

**Criterion #15660**:
`<element-citation>` elements have no attributes.

**Criterion #14559**:
`<element-citation>` contains element-only content with child elements from the set:
```
<article-title>~REF
<comment>
<date-in-citation>
<day>
<edition>
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
<source-title>
<uri>
<volume>
<year>
```

**Criterion #12492**:
For `<element-citation>` elements, there are one or zero child elements for each
possible tag, with the exception of `<pub-id>`, which can appear more than once.

**Criterion #13786**:
For `<element-citation>` elements, all child elements with the tag `<pub-id>` have different
values for the attribute `pub-id-type=`.

**Criterion #18428**:
The following elements under `<element-citation>` have no attributes and contain
text-only content:
```
<comment>
<fpage>
<isbn>
<issn>
<issue>
<lpage>
<publisher-loc>
<publisher-name>
<source-title>
<uri>
<volume>
```

**Criterion #10807**:
`<article-title>~REF` elements contain text-only content.

##### \<person-group>

**Criterion #18377**:
`<person-group>` elements have exactly one attribute, `person-group-type=`, with either the value
`"author"` or `"editor"`.

**Criterion #17091**:
`<person-group>` contains element-only content with child elements from the set:
```
<name>
<string-name>
<etal>
```

##### \<string-name>

**Criterion #18187**:
`<string-name>` elements have no attributes and contain text-only content.

##### \<etal>

**Criterion #16837**:
`<etal>` elements have no attributes and contain empty content.

**Criterion #14180**:
`<person-group>` elements have no more than one `<etal/>` child element.

##### \<year>, \<month>, and \<day> elements

**Criterion #13721**:
`<year>`, `<month>`, and `<day>` elements have no attributes.

**Criterion #17289**:
`<year>`, `<month>`, and `<day>` elements contain just an integer as content and do not have any non-digit
characters.

**Criterion #10430**:
Child elements `<year>`, `<month>`, and `<day>` appear at most once under their parent
element.

**Criterion #14321**:
Child element `<month>` appears only if `<year>` is present as a sibling element.

**Criterion #19206**:
Child element `<day>` appears only if `<month>` is present as a sibling element.

##### \<date-in-citation>

**Criterion #13166**:
`<date-in-citation>` has exactly one attribute, it is `content-type=`, and its value is
`"access-date"`.

**Criterion #11337**:
`<date-in-citation>` contains element-only content with child elements from the set:
```
<year>
<month>
<day>
```

##### \<edition>

**Criterion #18615**:
`<edition>` elements have no attributes.

**Criterion #11753**:
`<edition>` elements contain just an integer as content and do not have any non-digit
characters.

##### \<pub-id>

**Criterion #14308**:
`<pub-id>` elements have exactly one attribute, it is `pub-id-type=`, and it has the value `"doi"` or `"pmid"`.

**Criterion #15283**:
`<pub-id>` elements with the attribute value `pub-id-type="doi"` have text-only content
that starts with "10." and not "http".

**Criterion #10955**:
`<pub-id>` elements with the attribute value `pub-id-type="pmid"` have text-only content of a
valid PubMed Identification Number.

<!-- copybreak on -->


Changes
-------

### From Edition 1 to 2

#### HTML-like JATS Elements

The criteria for the following JATS elements from edition 1 have been replaced with criteria
for their corresponding XHTML elements in edition 2.

```
JATS          XHTML
-----------   -----
<bold>        <b>
<break/>      <br/>
<def-item>    <div> (under <dl>)
<def-list>    <dl>
<def>         <dd>
<disp-quote>  <blockquote>
<ext-link>    <a ref="external">
<italic>      <i>
<list>        <ul> <ol>
<monospace>   <tt>
<preformat>   <pre>
<sec>         <section>
<term>        <dt>
<title>       <h2> <h3> <h4> <h5> <h6>
<xref>        <a>   (only when xref ref-type is not "bibr")
```

#### JATS tags renamed for HTML/CSS-compatibility

The following JATS tags are renamed:

* `<body>` to `<article-body>`
* `<source>` to `<source-title>`
* `<ali:license_ref>` to `<license-ref>`

These renames achieve HTML/CSS/XML interoperability by satifiying criteria #10825,
#18620, and #14199.

#### \<li> and \<dd> child elements

Consistent with the HTML standard,
`<li>` and `<dd>` child elements can be any elements from the set `{P_LEVEL}`.
This is an intentional non-alignment with the NISO JATS standard.
The reference XSL transform file will transform Baseprint XML to JATS XML,
moving required child elements to be under non-HTML JATS `<p>` child elements as required by
NISO JATS.

#### Elimination of non-HTML JATS \<p>

The JATS-specific `<p>~WRAPPER` element variety and all related criteria have been
eliminated or replaced.
The `<p>` element in this edition 2 is equivalent to the `<p>~HTML` element variety
of edition 1.

The `<p>` element in Baseprint XML is now aligned with the HTML standard.
This is an intentional non-alignment with the NISO JATS standard.
The reference XSL transform file will transform Baseprint XML to JATS XML that does
include non-HTML-standard JATS `<p>` elements required by the NISO standard.

#### Stronger content criteria

\<article-title>
: changed to only contain the elements `<b>`, `<i>`, `<sub>`, and `<sup>`.
  See [Discussion #19 on GitHub](
  https://github.com/singlesourcepub/baseprints/discussions/19) for the rationale.

\<abstract>
: content criteria require only `{P_LEVEL}` child elements and no subsections.

\<copyright-statement>
: changed to contain `{COPYTEXT}` instead of `{HYPERTEXT}`.

\<license-p>
: changed to contain `{COPYTEXT}` instead of `{HYPERTEXT}`.

#### Elimination ref-list/title

`<title>` under `<ref-list>` has been removed. This tag name is also used in HTML.
This title has only been set to "References" in both all existing Baseprint documents
and the vast majority of recent PubMed Central JATS XML files.
This element also resulted in a more complex parsing model being at the same level as a
sequence of `<ref>` child elements.
If customization of reference title is ever needed, a different tag name and
location are likely to be chosen for Baseprint XML.

#### Misc

* `xmlns:xlink` namespace no longer needed since `<ext-link>` replaced with `<a>`
* `<xref>` element in edition 2 is equivalent to `<xref>~CITE` of edition 1
* JATS `<elocation-id>` eliminated (use `<fpage>` for better CSLJSON interoperability)
