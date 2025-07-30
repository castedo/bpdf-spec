<!-- source text for future edition -->

### XML Basics

* `<table-wrap>`
* `<table>`
* `<tbody>`
* `<thead>`


### Minimal attributes

* `<table-wrap>`
* `<table>`
* `<tbody>`
* `<thead>`


### `<body>` element tree

```
<table-wrap>
```


#### Paragraph elements

```
<table-wrap>
```


#### Table elements

**Criterion**:
`<table-wrap>` elements contains a single `<table>` element.

**Criterion**:
`<table>` child elements are `<thead>` or `<tbody>` elements.

**Criterion**:
`<thead>` and `<tbody>` child elements are `<tr>` elements.

**Criterion**:
`<tr>` child elements are `<th>` or `<td>` elements.

**Criterion**:
`<th>` and `<td>` elements have all child elements with constraint `$P_CHILD`.

**Criterion**:
`<th>` and `<td>` attributes `align=` have values `"left"`, `"center"`, `"right"`, or
`justify`.



Interoperability Issues
-----------------------

* HTML `<table>` has `<thead>` before `<tbody>`
* HTML `<table>` probably (?) has exactly one `<tbody>` and at most one `<thead>`
