# Introduction

This guidelines document is a joint effort between the (mostly) Nordic agencies dedicated to providing accessible literature in different formats – e.g. talking books, braille, and accessible e-books – to children and adults with various reading impairments or special needs. The participating organisations are [Celia](https://www.celia.fi/), [Dedicon](https://www.dedicon.nl/), [HBS](https://hbs.is/), [MTM](https://mtm.se), [NLB](https://www.nlb.no/), [Nota](https://nota.dk/), [SBS](https://www.sbs.ch/), [SPSM](https://www.spsm.se/), and [Statped](http://statped.no/).

Making book content accessible starts in well-structured and granular semantic markup using available markup and accessibility standards. For the Nordic agencies, a starting point in their production of accessible literature is a base production file in EPUB 3, from which further transformations are made, and/or additional layers of accessibility features are added. This guidelines document collects instructions for the application of EPUB 3 [[epub-33]] and EPUB Accessibility 1.1 [[epub-a11y-11]], bridging the gap between general standards and the local context(s) of the Nordic agencies.

The target audience of the document is mainly the Nordic agencies’ contracted EPUB 3 suppliers, but the guidelines will also be used by staff at the Nordic agencies, other vendors and interested parties, etc.

## For readers looking for general e-book accessibility advice

Readers should note that these guidelines build on available standards and best practices for accessible e-book production documented elsewhere. This document does not provide general e-book accessibility advice, please have a look at the [DAISY Consortium's Accessible Publishing Knowledge Base](https://kb.daisy.org/publishing/docs/) for a comprehensive resource on current best practices for e-book accessibility in general. Many of the techniques used in this document are based on DAISY Accessible Publishing Knowledge Base examples and guidance. The Nordic agencies are indebted to the [DAISY Consortium](https://daisy.org/) for providing this valuable resource.

<div class="note" title="About WCAG compliance">

Please note that some of the instructions given here do not provide sufficient accessible markup to satisfy the accessibility requirements of the Web Content Accessibility Guidelines [[WCAG22]], even on the [base conformance level (A)](https://www.w3.org/TR/WCAG22/#cc1). These sections presume post-markup editing to make productions fully WCAG-compliant. A note of warning is attached to these sections.

</div>

## Situating the Nordic Guidelines in the World of Specifications

As described above, the Nordic Guidelines work as an application of higher-level standards for the purpose of accessible EPUB 3 production at the Nordic agencies (henceforth ”the Ordering Agency/Agencies”).

The application of the Nordic Guidelines can also be further specified on lower levels, e.g. instructions on a per-Ordering Agency and/or per-title basis.

The different levels of specification can be expressed as the following hierarchy:

1. High-level specifications, e.g. [[epub-33]], [[epub-a11y-11]], [[HTML5]] etc.
2. The Nordic Guidelines (this document)
3. General Ordering Agency-specific guidelines (usually these build on the Nordic Guidelines, but might also contain some deviations from them)
4. Title-specific instructions, usually expressed in the form of Editing Instructions, containing further guidance on how to treat a specific title

Note that the forms of levels 3 and 4 will vary between the Ordering Agencies, and their standardisation is outside the scope of this document.

# Format Requirements

## Required EPUB Standard

Suppliers are required to refer to the specifications provided in version 3.3 of the EPUB standard [[epub-33]].

## Container

The EPUB [container file](https://www.w3.org/TR/epub-33/#sec-container-metainf-container.xml) must be given the production number provided with the order and correspond with the identifier stored in the [^dc:identifier^] located in the [^package^] metadata.

The container archive is required to have the `.epub` extension. The file extension must be lowercase.

Note that the [`mimetype` file](https://www.w3.org/TR/epub-33/#sec-zip-container-mime) must be archived as the first file in the container.

### META-INF

The container file must identify no more than one media alternative, unless indicated otherwise by the Ordering Agency.

The container file shall look like this, unless indicated otherwise:

<aside class="example" title="The container file">

```xml
<?xml version="1.0" encoding="utf-8" ?>
<container version="1.0" xmlns="urn:oasis:names:tc:opendocument:xmlns:container">
  <rootfiles>
    <rootfile full-path="EPUB/package.opf" media-type="application/oebps-package+xml"/>
  </rootfiles>
</container>
```

</aside>

No other files, optional or otherwise, are allowed in the [META-INF directory](https://www.w3.org/TR/epub-33/#sec-container-metainf) unless specifically indicated by the Ordering Agency.

## Publication Resources

All [=publication resources=] are required to be located in a directory called `EPUB`. Publication resources other than the [=package document=] (`.opf`) and [=XHTML content documents=] are to be located in dedicated resource sub-directories.

## Package Document

The name of the [=package document=] file is required to be `package.opf`. Suppliers are required to use the file extension `.opf` for the package document.

The following xml declaration must be placed at the first line of the document:

```xml
<?xml version="1.0" encoding="utf-8" ?>
```

Suppliers are required to apply the following attributes and values to the [^package^] element:

- `xmlns="http://www.idpf.org/2007/opf"`
- `xmlns:dc="http://purl.org/dc/elements/1.1/"`
- `prefix="nordic: http://www.mtm.se/epub/"`
- `version="3.0"`
- `xml:lang="xx"` (the publication language code)
- `unique-identifier="pub-identifier"`

Required child elements to the package element are:

- [^metadata^]
- [^manifest^]
- [^spine^]

### Metadata

The following metadata are required to be placed in the [^metadata^] element:

```xml
<dc:title id="title">_[the title of the publication]_</dc:title>
<dc:language>_[language code for the main language]_</dc:language>
<dc:identifier id="pub-identifier">_[production UID provided by the ordering agency]_</dc:identifier>
<dc:source>_[ISBN or ISSN of the publication]_</dc:source>
<dc:creator>_[author of the publication (one element for each author)]_</dc:creator>
<dc:format>application/epub+zip</dc:format>
<dc:publisher>_[the ordering agency]_</dc:publisher>
<dc:date>_[date of completion]_</dc:date>
<meta property="dcterms:modified"> _[date of completion]_ </meta>
<meta property="nordic:supplier"> _[company name of supplier]_ </meta>
<meta property="nordic:guidelines">2024-1</meta>
```

Note that the default value to use in `dc:publisher` [[dcterms]] is the shorthand for the Ordering Agency, e.g. "NLB", "MTM", etc. Optionally, the Ordering Agency can request the original source publisher to be expressed in `<meta property="dc:publisher.original">`.

If the source material does not have an ISBN, ISSN or any other systematic source identifier the content of the `dc:source` element [[dcterms]] will be a string based on whatever available information about the source there is (publisher, year of publication etc.). This will be provided by the Ordering Agency via Editing Instructions.  

#### Accessibility Metadata

The supplier is also expected to add accessibility metadata. Some Ordering Agencies may, however, have their own workflows for creating this metadata. If it is not to be included, this will be indicated in agency-specific guidelines. The agency may also give instructions about accessibility metadata in title-specific editing instructions.

A full description of accessibility metadata in EPUB can be found in sections [2. Discoverability](https://www.w3.org/TR/epub-a11y-11/#sec-discovery) and [3.5 Conformance reporting](https://www.w3.org/TR/epub-a11y-11/#sec-discovery) of EPUB Accessibility 1.1 [[epub-a11y-11]] and in [[schemaprop]]. See also the Daisy Accessible Publishing Knowledge Base, [Metadata](http://kb.daisy.org/publishing/docs/metadata/).

The metadata will vary depending on the content and properties of the publication. The examples below show what it may look like for a simple and a more complex book.

<aside class="example" title="Metadata for a simple book without images">

```xml
<meta property="schema:accessMode">textual</meta>
<meta property="schema:accessModeSufficient">textual</meta>
<meta property="schema:accessibilityFeature">displayTransformability</meta>
<meta property="schema:accessibilityFeature">structuralNavigation</meta>
<meta property="schema:accessibilityFeature">tableOfContents</meta>
<meta property="schema:accessibilityFeature">readingOrder</meta>
<meta property="schema:accessibilityHazard">none</meta>
<link rel="dcterms:conformsTo" href="https://format.mtm.se/nordic_epub/2024-1"/><!--Check url-->
<meta property="a11y:certifiedBy">[the ordering agency's name]</meta>
```

</aside>

<aside class="example" title="Metadata for a complex book with described images and MathML markup">

```xml
<meta property="schema:accessMode">textual</meta>
<meta property="schema:accessMode">visual</meta>
<meta property="schema:accessModeSufficient">textual</meta>
<meta property="schema:accessibilityFeature">displayTransformability</meta>
<meta property="schema:accessibilityFeature">structuralNavigation</meta>
<meta property="schema:accessibilityFeature">tableOfContents</meta>
<meta property="schema:accessibilityFeature">readingOrder</meta>
<meta property="schema:accessibilityFeature">alternativeText</meta>
<meta property="schema:accessibilityFeature">longDescription</meta>
<meta property="schema:accessibilityFeature">MathML</meta>
<meta property="schema:accessibilityHazard">none</meta>
<link rel="dcterms:conformsTo" href="https://format.mtm.se/nordic_epub/2024-1"/>
<meta property="a11y:certifiedBy">[the ordering agency's name]</meta>
```

</aside>

As with `dc:publisher`, the default value of `a11y:certifiedBy` [[epub-a11y-11]] is the shorthand for the Ordering Agency.

Please note that the version number forming the end of the `dcterms:conformsTo` URL must match the value of the `nordic:guidelines` property, i.e. (for this version of the guidelines) "2024-1".

<div class="note" title="Note on accessMode">

The `visual` value indicates that the book contains visual content such as images, graphics or video that is relevant to understanding the content. The value must not be set if the only visual elements are decorative images, the cover image or corporate logos.

</div>

If the book contains page break markers and a page list, the following metadata must be provided:

<aside class="example" title="Metadata for page-related content">

```xml
<meta property="schema:accessibilityFeature">pageBreakMarkers</meta>
<meta property="schema:accessibilityFeature">pageNavigation</meta>
<meta property="pageBreakSource">urn:isbn:xxxxxxxxxxxxx</meta>
```

</aside>

The [`pageBreakSource`](https://www.w3.org/community/reports/publishingcg/CG-FINAL-page-source-id-20230314/#pageBreakSource) property [[pagesourceid]] identifies the pagination source, typically using the ISBN of a print edition. See also the Daisy Accessible Publishing Knwoledge Base, [Page Source](http://kb.daisy.org/publishing/docs/navigation/pagesrc.html) for more information and examples.

The `schema:accessibilitySummary` property can be used to provide information taht complements, but does not duplicate, the other metadata. The summary also describes any known deficiencies. It should be used only if indicated by the Ordering Agency, which will provide the summary text.

#### Optional Increased Title and Creator Metadata Granularity

The Ordering Agency may request increased metadata granularity for the representation of title and creator details, making use of a linked meta element refining the character of the main metadata element.

The following is an example of a more granular way of expressing title metadata, separating main title, subtitle, and edition title statements from each other:

<aside class="example" title="Granular title metadata">

```xml
<dc:title id="maintitle"> _[the main title of the publication]_ </dc:title>
<meta refines="#maintitle" property="title-type">main</meta>
<dc:title id="subtitle"> _[the subtitle of the publication]_ </dc:title>
<meta refines="#subtitle" property="title-type">subtitle</meta>
<dc:title id="edition"> _[the edition part of publication title, e.g. "1st ed."</dc:title>
<meta refines="#edition" property="title-type">edition</meta>
```

</aside>

Similarly, creator/author information can be expressed with explicit refinement of the creator role of each author, as in the following example:

<aside class="example" title="Granular creator metadata">

```xml
<dc:creator id="creator1"> _[coauthor of the publication]_ </dc:creator>
<meta refines="#creator1" property="role" scheme="marc:relators" id="role">aut</meta>
<dc:creator id="creator2"> _[coauthor of the publication]_ </dc:creator>
<meta refines="#creator1" property="role" scheme="marc:relators" id="role">aut</meta>
<dc:creator id="creator3"> _[illustrator of the publication]_ </dc:creator>
<meta refines="#creator1" property="role" scheme="marc:relators" id="role">ill</meta>
```

</aside>

The codes to use as values for the creator role are taken from the [MARC relators vocabulary](https://id.loc.gov/vocabulary/relators.html).

#### Optional Additional Metadata

Additional metadata for inclusion can be supplied by the Ordering Agency on a per-title or per-Agency basis.

### Manifest

In [^manifest^] all publication resources are declared. Each resource is declared using the [^item^] element with required attributes [`id`](https://www.w3.org/TR/epub-33/#attrdef-id), [`href`](https://www.w3.org/TR/epub-33/#attrdef-href) and [`media-type`](https://www.w3.org/TR/epub-33/#attrdef-media-type). Some items must also have a [`properties`](https://www.w3.org/TR/epub-33/#attrdef-properties) attribute.

The `id` attribute is a unique identifier used to address a particular publication resource. The only formal requirement here is its uniqueness, but the Ordering Agency will require certain naming conventions to be applied.

The `href` attribute is simply a path to the publication resource. The path will be relative to where the package document is located.

The `media-type` attribute specifies what type of resource it is and what format. Here are some of the more common examples:

- `application/xhtml+xml`
- `text/css`
- `image/jpeg`
- `application/x-dtbncx+xml`
- `application/javascript`

Refer to the [core media types table of EPUB 3.3](https://www.w3.org/TR/epub-33/#sec-core-media-types) for additional information about various media types.

Some resources have additional properties that need to be identified in the corresponding item in order for a reading system to be able to implement these properties. This is done by using the properties attribute. Common examples are:

- `nav` for the `xhtml` navigation document
- `cover-image` for the cover image
- `scripted` for any content document where javascripts are being used
- `mathml` for any content document containing MathML

Refer to the [manifest properties vocabulary of EPUB 3.3](https://www.w3.org/TR/epub-33/#app-item-properties-vocab) for additional information about the properties attribute.

### Spine

In the [^spine^] all content documents are listed in the correct reading order. This is done by using an [^itemref^] element for each content document and simply listing them in the desired order. The [^itemref^] element is associated with the corresponding [^item^] for the content document in the [^manifest^] by using the [`idref`](https://www.w3.org/TR/epub-33/#attrdef-itemref-idref) attribute and setting the value to the [`id`](https://www.w3.org/TR/epub-33/#sec-item-elem) attribute of the [^item^] element. The `idref` attribute is a required attribute and the id it refers to must be unique. Here is an example:

<aside class="example" title="manifest and spine items linking">
In [^manifest^]:

```xml
<item id="item_7" href="007-chapter.xhtml" media-type="application/xhtml+xml"/>
```

In [^spine^]:

```xml
<itemref idref="item_7"/>
```

</aside>

[^itemref^] has an optional attribute called [`linear`](https://www.w3.org/TR/epub-33/#attrdef-itemref-linear). The `linear` attribute can have the values `yes` (default) or `no`. If the attribute is omitted it is set to `yes`. The `linear` attribute indicates whether the referenced item contains content that contributes to the primary reading order and has to be read sequentially (`yes`) or auxiliary content that enhances or augments the primary content and can be accessed out of sequence (`no`). Examples of auxiliary content include: notes, descriptions and answer keys.

As a general rule `linear="no"` should only be applied to the cover file of a title. This is to ensure a reader does not miss out on any of the content, no matter which reading system they are using. Agency-specific guidelines might have other requirements than this.

If a fall-back [`ncx` navigation document](https://www.w3.org/TR/epub-33/#sec-opf2-ncx) is included in the EPUB package, this is required to be referenced by adding the `toc` attribute to the [^spine^] element and assign as value the `id` attribute of the [^item^] referring to the ncx file, like this:

<aside class="example" title="Spine referencing in case of an ncx file">

```xml
<spine toc="_[id value of the manifest item that refers to the ncx file]_">
```

</aside>

## Content Documents

### XHTML

The [=xhtml content document=] files specified by [[epub-33]] are based on [[HTML5]]. Suppliers are required to use the extension `.xhtml`.

#### XML Declaration and Encoding

The following XML declaration must be used:

```xml
<?xml version="1.0" encoding="utf-8"?>
```

#### Document Type Declaration

The following document type declaration must be included:

```html
<!DOCTYPE html>
```

#### HTML Root Attributes

Suppliers are required to include the following attributes on the [^html^] root element:

- `xmlns` – default namespace
- `xmlns:epub` – EPUB namespace
- `xml:lang` – XML language definition
- `lang` – HTML language definition

#### Namespaces

The following namespace values are required to be applied to the namespace attributes:

- `xmlns="http://www.w3.org/1999/xhtml"`
- `xmlns:epub="http://www.idpf.org/2007/ops"`

#### Language Definition

Suppliers are required to identify the primary language of each content document using the `xml:lang` and `lang` attributes. On how to tag language change within a document, see section [Language Tagging](#language-tagging).

If not requested otherwise by the Ordering Agency, language should be coded using values from the [IANA registry of valid language codes](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry).

Please note that the most granular language tag should be used, e.g. the macrolanguage value `no` for Norwegian could be used, but only in instances where the sublanguages Norwegian Nynorsk (`nn`) or Norwegian Bokmål (`nb`) are not known.

The accuracy of language coding is vital, and Suppliers are instructed to contact the Ordering Agency in case of any uncertainties.

### Title

The [^title^] element of every [=xhtml content document=] must match the main heading of the content document in question. For front- and backmatter content, if the document does not have a heading, the [`aria-label`](https://www.w3.org/TR/wai-aria/#aria-label) [[wai-aria]] attribute value of the main [^section^] element is to be used instead. If bodymatter content does not have headings, use the three first words of the content, followed by a space and en ellipsis character (not three points).

### Metadata

The content documents are required to contain two [^meta^] elements.

```html
<meta name="dc:identifier" content="_[production UID]_"/>
<meta name="viewport" content="width=device-width"/>
```

The production UID must match the [^dc:identifier^] of the [=package document=].

## Navigation Documents

### EPUB 3.2 Navigation Document

The principle navigation document of the EPUB package is the `xhtml` file with the `properties` attribute set to nav in the `<manifest>` section of the package document.  For matters of convenience mostly, this file is required to be named `nav.xhtml`.

Section 5.2.1 of this document contains language specific headings for the `<nav>` sections of the navigation document that are listed below.

#### The Table Of Contents

The first required `<nav>` element in the file is for the main table of contents. The `<nav>` element is required to have a `<h1>` as the first child element. The `<nav>` element must have the `aria-labelledby` attribute set to the `id` of the `<h1>` element. The `role` and `epub:type` elements must be set as follows:

```html
<nav role="doc-toc" aria-labelledby="n1" epub:type="toc">
<h1 id="n1">Contents</h1>
```

All headings in the main body of text must be included in the `<nav role="doc-toc"...>` element and the heading levels must be implied through nesting. Headings of sidebars, text boxes or other secondary content should not be included, unless specific instructions are given by the Ordering Agency.

The title page is referenced using the label "Title page" in the language of the publication (see section [aria-label and TOC Values](#aria-label-and-toc-values)). The book title is not included in the TOC.

Some headings consist of a main heading and a subtitle grouped in an `<hgroup>` element (see section [Headings](#headings) for details on markup). The subtitle should be included in the same TOC entry as the main heading. The text of both heading elements is concatenated into one string with a word space separating them. If the main heading does not end in a suitable punctuation mark, a period is also added:

```html
<li><a href="...">Main heading. Subtitle</a></li>
```

The links must always reference the corresponding sectioning element for the heading in the content file, not the `h[x]` element directly. This is usually a `<section>` element, but can also be `<aside>` or any other sectioning element. Thus, in the example below, the link should point to the id "level3_2" in that content file:

```html
<section aria-labelledby="h3_2" id="level3_2">
	<h3 id="h3_2">Det mytiska Norden</h3>
```

A section without any heading must be referenced using its `aria-label` value.

Note that this is not a representation of the table of contents in the source material. Only headings, or references using the `aria-label` value of the corresponding `<section>` element, should be included.

If, for some reason, unlinked content must be added to the  table of contents, this must be marked up with `<span class="toc-unlinked">`, instead of the `<a>` element. Unlinked entries must only be added if specific instructions are given by The Ordering Agency.

#### Page List

If the source material contains pagination, the next required `<nav>` element is: 

```html
<nav role="doc-pagelist" aria-labelledby="n2" epub:type="page-list">
<h1 id="n2">Pages</h1>
```

See the following link for further information:

[http://kb.daisy.org/publishing/docs/navigation/pagelist.html](http://kb.daisy.org/publishing/docs/navigation/pagelist.html)

If no pagination occurs in the source material this may be omitted.

#### Landmarks

The landmarks `<nav>` element is optional, and should be included only if the Ordering Agency requests it.

```html
<nav aria-labelledby="n3" epub:type="landmarks">
<h1 id="n3">Landmarks</h1>
```

The Ordering Agency will give instructions on which sections to include and how to label them in the landmarks navigation list. For examples and further information, see the sections about EPUB landmarks in the [EPUB 3.3 specification](https://www.w3.org/TR/epub-33/#sec-nav-landmarks) and [EPUB Accessibility Techniques 1.1](https://www.w3.org/TR/epub-a11y-tech-11/#meta-006).

It is recommended that the list of landmarks include a link to the start of the bodymatter as well as to any navigation aids or important reference sections in the front- and backmatter. It will typically include the following, when applicable:

- Frontmatter:
    - Table of Contents
    - List of Images
    - List of Tables
- Bodymatter:
    - Start of content/Start reading
- Backmatter:
    - Notes
    - Bibliography
    - Index
    - Answers
    - Glossary

Introductory sections such as a prologue, preface, foreword or introduction can also be included in the landmarks. It should be noted, however, that they are considered bodymatter. In the landmarks `<nav>`, the first bodymatter section should be referenced using `epub:type="bodymatter"` and a label indicating that it is the start of the main content, regardless of any other applicable `epub:type` value.

### NCX Navigation Document

The EPUB package may include an `ncx` navigation document as fall-back for older reading systems that have not implemented functionality for the EPUB 3 navigation document. It is not required by the EPUB specification, but it may be requested by the Ordering Agency. No `ncx` file should be included unless specifically requested. If requested, the file is required to be named `nav.ncx`.

Refer to [http://www.idpf.org/epub/20/spec/OPF_2.0.1_draft.htm#Section2.4](http://www.idpf.org/epub/20/spec/OPF_2.0.1_draft.htm#Section2.4) for information about how the `ncx` file should be formed. Required elements are:

- `<head>`
- `<docTitle>`
- `<navMap>`
- `<pageList>`

The `<head>` element must contain the following `<meta>` element:

```html
<meta name="dtb:uid" content=" _[production UID provided by the ordering agency]_ "/>
```

All headings must be included in the `<navMap>` and the heading levels must be implied through nesting.

## Images

Image content is required to be captured in the `jpeg` or `png` formats. The format extension is required to be `.jpg` or `.png`. Photos, drawings and illustrations must be captured in the `jpeg` format. For line graphics, charts and diagrams the preferred format is `png`.

Suppliers are required to maintain the highest possible image quality. The quality
requirements are the following:

- The aspect ratio of the original image must always be maintained.
- Colours must be reproduced and rendered accurately.
- The image must be free of any compression artefacts or any distortion effects arising from scanning.
- Text in images, line graphics and small detail must be crisp and fully legible, or with no degradation in legibility in comparison with the original image.
- If the `jpeg` format is used, the quality setting is required to be set at around 90% (or a corresponding setting, if a different scale of measurement is used). A setting of 100% is usually unnecessary as it yields substantially larger files with little or no visible improvement of quality. If, however, there would be visible difference in quality between 90% and a higher setting, then the higher setting should be used.

### Image File Naming and Directory Structure

Image files are required to be stored in a folder named _images_ placed at the same level relative to the package document. The images folder must not contain subfolders.

Image files should be named according to the following file naming convention:

```
[UID]-[XXX].[png|jpg]
```

The _UID_ must be identical to the value of the `dc:identifier` metadata element. _XXX_ is a three-digit numeric string corresponding to the order of the image in the content, with leading zeros as needed. (If the number of images in a single book exceeds 999, a four-digit number should be used.)

Example:

```
X41001A-013.png
```

An exception to this is the cover image, which should be named simply `cover.png` or `cover.jpg`.

### Image id:s for loi
In cases where a title contains a list of illustrations (`loi`), or where a list of ilustrations should be included in the `nav.xhtml`-file , the figure element  should be provided with an id. These id:s should be named`"fig-XXX"` in sequential order, where XXX is the number of the figure in the file.
Instances may occur where id:s are required for images. In those cases the id should be `"img-XXX"` and follow the same pattern as for figures.

### Resizing of Images

The maximum image size is set to 800 pixels on the longest side of the image, unless an increase in the size of an image is required to achieve the legibility of text rich images, see point 4 above, or other small details.

In those circumstances where this requirement conflicts with requirements for legibility, the Supplier is required to contact the Ordering Agency.

### Image Editing

Images are required to be captured exactly as they are in the scans or publisher files provided by the Ordering Agency. There is generally no need for editing images to enhance them above the quality of the originals (e.g. cloning to smooth edge of images spanning over spread, enhancing bad scans or old photos with bad quality already in print, etc.).

When whole sections or parts of text is placed on top of a background image, the text may need to be edited out of the background, if possible. However, no advanced restoration of lost image details are required. Note that if a publisher file is provided, images and text may have been placed in different layers and the image can be extracted without any text.

## CSS

Suppliers are required to include the standard `css` file issued by the Ordering Agency. The `css` file is required to be stored in a folder named _css_ and placed at the same level relative to the package document.

The `<link>` element is required to be applied to the relevant content documents.

## Fonts

Fonts present in PDF source material must not be included in the EPUB, unless specifically indicated by the Ordering Agency.

Fonts, when present, must be stored in a folder named _fonts_ and placed at the same level relative to the package document.

## Javascript

Javascript files requested by the Ordering Agency are required to be stored in a folder named _javascript_ and placed on the same level relative to the package document.

The `<script>` element is required to be applied to the relevant content documents.

Suppliers must not include javascript files in the EPUB unless specifically instructed to do so by the Ordering Agency.

## Validation

Validation of EPUB files should be done using EPUBCheck for structural validity and conformance to the EPUB standard and the Ace tool for accessibility evaluation. The latest distributions of these tools should be used. The tools can be found here:

- [https://github.com/w3c/epubcheck/releases](https://github.com/w3c/epubcheck/releases)
- [https://inclusivepublishing.org/toolbox/accessibility-checker/getting-started/](https://inclusivepublishing.org/toolbox/accessibility-checker/getting-started/)

The Ordering Agencies have developed a specific validation tool based on these guidelines. This tool, or any similar validation-tool provided by the agencies based on the same ruleset, should be the primary validation method and can be found here:

- [http://nlbdev.github.io/nordic-epub3-dtbook-migrator/](http://nlbdev.github.io/nordic-epub3-dtbook-migrator/)

# General Requirements for Content Documents

## Structural Divisons and Semantics

The EPUB Content File structure specified in these guidelines is generally made up of a multi-page HTML file set. Major divisions of the publication are to be captured in individual XHTML content files. The individual content files will typically correspond to _Part_ or _Chapter_ divisions of the book. Other major book components such as colophon, index or appendix, which can be found in front-matter and back-matter, will normally be stored in separate files as well.

The structural divisions of the publication are required to be semantically inflected by wrapping every structural part of the main text body in `<section>` elements, with proper nesting of subsections. Usually, a subsection ends whenever a new heading of the same level or higher occurs in the source material. However, there may be situations where a `<section>` element needs to be closed sooner. If so, this should be specified in the Editing Instructions given by the Ordering Agency. If the structure of the source material is unclear and the Editing Instructions do not provide answers, please contact the Ordering Agency. 

The semantic role of each content document must be specified, when possible. This is done by adding the `role` and `epub:type` attributes to the top level `<section>` element of each content file. The available `role` attribute values are listed in the [Digital Publishing WAI-ARIA Module 1.1](https://www.w3.org/TR/dpub-aria-1.1/#roles).

When the content file can be matched to one of the roles listed, the top level `<section>` element is required to have the `role` attribute with that matching value set. If no suitable role exists the `role` attribute may be omitted.  

The `epub:type` attribute is also required for the top level `<section>` element of each content file and must first contain one of the following partition values:

- `cover`
- `frontmatter`
- `bodymatter`
- `backmatter`

The `frontmatter`, `bodymatter` and `backmatter` partition values must, where possible, be combined with a divisioning or sectioning value found in the [EPUB 3 Structural Semantics Vocabulary 1.1](https://www.w3.org/TR/epub-ssv-11/). Note that according to this vocabulary, introductory sections such as foreword, introduction, preface and prologue are typically part of the publication bodymatter.

If no matching divisioning or sectioning value can be found, the `epub:type` will be set to only the appropriate partition value. Also, the content file containing the cover image of the printed book will have simply `epub:type="cover"`.

If an EPUB file contains both parts and chapters, and each part and all its chapters is contained in a single content file, the above is also required for the second level `<section>` elements, which will contain the chapters.

Furthermore, the top-level `<section>` element for every content file is required to have a label. When the `<section>` has a heading it will serve as a label, but must be associated with the `<section>` element.  This is done by using the `aria-labelledby` attribute and setting the `id` of the associated header as value:

```html
<section role="doc-chapter" aria-labelledby="hd01" epub:type="bodymatter chapter">
	<h1 id="hd01">Chapter 1</h1>
		...
</section>
```

Note that for the most part it will only by sections with `<h1>` headings that must be done this way, but if a production is split into parts, chapters will have `<h2>` headings and these are the top-level headings of those content documents.

Note that if the source material for an EPUB production is an EPUB file, or any other HTML or XML based material, it can not be assumed that the structural semantics is already correct. Proper analysis of the structural division and semantics must always be performed by the Supplier in accordance with this document.

### Subsections
No sub-sections of any content document should have an `aria-label` or `aria-labelledby` attribute, unless specific instructions are given by the Ordering Agency.

### Untitled sections
If a major section, e.g. a chapter, is untitled, the `aria-label` attribute is required to be used instead of a regular heading element, and have an appropriate label assigned to it. The value of the label is either a default value for specific standard sections, a standardised value derived from the start of the first paragraph in the section, or a custom value upon request from the Ordering Agency.

In rare cases, the same method can be used to label untitled subsections. Such cases will be accompanied by Editing Instructios specifically requesting this treatment.

#### Default `aria-label` values for standard sections
The following table lists default `aria-label` values for standard sections.

| Section identification   | Default `aria-label` value |
|--------------------------|----------------------------|
| `@epub-type="cover"`     | Cover                      |
| `@class="frontcover"`    | Front cover                |
| `@class="backcover"`     | Back cover                 |
| `@class="leftflap"`      | Jacket left flap           |
| `@class="rightflap"`     | Jacket right flap          |
| `@role="doc-colophon"`   | Publisher information      |
| `@role="doc-dedication"` | Dedication                 |
| `@role="doc-epigraph"`   | Epigraph                   |
| `@role="doc-toc"`        | Contents                   |

Section 5.2.2 of this document contains language specific values.

##### `aria-label` values using the first three words
If nothing else is mentioned in the accompanying Editing Instructions for the specific title, untitled chapter sections should be named with `aria-label` using the first three words of the chapter text, followed by a space and an ellipsis, e.g. `aria-label="Det var längesedan …"` for a chapter section where the first three words of the body text are "Det var längesedan".

##### Custom `aria-label` values
If none of the two methods above fits, the Ordering Agency may request custom `aria-label` values in Editing Instructions.

## File Naming Convention

The basic scheme for naming individual files is:

```
[UID]-[XXX]-[role].xhtml
```

The _UID_ must be identical to the value of the `dc:identifier` metadata element. _XXX_ is a three-digit numeric string corresponding to the order in the `<spine>`, with leading zeros as needed. _role_ corresponds to the ARIA `role` of the main section element in the content file, minus the "`doc-`" part. In the absence of an ARIA role for the top-level section of the content document, the value from the `epub:type` attribute should be used instead. In the case of multiple `epub:type` (e.g. `frontmatter titlepage`)values, the most specific (e.g. `titlepage`) should be used.

Example:

```
X41001A-010-chapter.xhtml
```

## Special Content Requirements

Some content files have certain contents that are required to be included and marked up correctly.

### Cover and back cover

The cover should be captured in a separate file from the back cover and only contain the cover-image. The `linear="no"` attribute must be applied to the `itemref` element in the package spine corresponding to the cover file. 

The front cover, when available, must be captured as a `.jpg` or `.png` image file and given the name `cover.jpg/png`. The `properties="cover-image"` attribute must be applied to the manifest item element corresponding to this image. The `<img>` element referencing the cover image must have `aria-role` set to `"doc-cover"`.

The back cover should be captured as the first frontmatter-file in the title with the right- and left flaps as sub sections. The following class attributes must applied to the `<section>` and child `<section>` elements where appropriate:
- `<section class="backcover">`
- `<section class="leftflap">`
- `<section class="rightflap">`

The placement of the back cover file might vary between agency specific guidelines.

Ordering Agencies may require a legalpage or other frontmatter-content to be inserted into the title. The specifications and placing of this will be up to the Ordering Agency.


### Title Page

The publication’s title must be included in an `<h1>` element with `class="title"` and `epub:type="title"`.

```html
<h1 epub:type="title" class="title" id="booktitle">Title</h1>
```

For subtitles, use a `<p>` tag with `epub:type="subtitle"` and `role="doc-subtitle"`. Group the title and subtitle in an `<hgroup>` element.

```html
<hgroup>
    <h1 epub:type="title" class="title" id="booktitle">Main title</h1>
    <p epub:type="subtitle" role="doc-subtitle">Subtitle</p>
</hgroup>
````

For the author of the publication, use `<p class="docauthor">`. For titles where there are more than one author, repeat this for each instance.

In some titles, more detailed classes for titlepage can be required: 

`<p class="seriestitle>`
`<p class="contributor">`
`<p class="publisher">`
`<figure class="logo">`

`<p class="contributor">` represents several different types of contributors such as illustrator, editor or translator. 
`<p class="seriestitle>` and `<p class="publisher">` should mostly be used when requested by the Ordering Agency.


### Parts

Some publications are divided into parts, each containing a number of chapters. By default, the part heading and any associated contents must be placed in a separate content file and the `<section>` element associated with the part heading must be closed at the end of that file. Each chapter of the part must then have its own content file. Note that chapter headings must be marked up as `<h2>`, even though they are the first heading of its content file. Semantic attributes, `role` and `epub:type`, must be given to the `<section>` elements of both part and chapters.

Ordering Agencies might give instructions to keep whole parts in single content files, if the size of the content is small enough to not inhibit the performance of reading systems. The part heading will be the `<h1>` and the chapter headings `<h2>` and the `<section>` element associated with the part heading will wrap all the contained chapters. Of course, semantic attributes, `role` and `epub:type`, must still be given to the `<section>` elements of both part and chapters. Note that this alternative option is only to be used if cleared by the Ordering Agency.

## Mark-up Requirements

These guidelines will not give highly detailed descriptions of how to correctly handle general content. Common recommendations for making valid and accessible HTML content will apply. As a general rule, the simple solution is almost always the best solution. Using common elements like `<p>`, `<blockquote>`, `<aside>`, `<ul>`, `<ol>`, `<dl>`, `<table>`, `<figure>` etc. and the common structural elements like `<section>` and headings will almost always be sufficient. More specialised elements, or special attributes, may be needed occasionally, though. Some of the more common cases will be described below, and more obscure ones will be covered specifically in Editing Instructions.

For further information about the common HTML elements and their attributes, please refer to any online resource on HTML 5, for instance:

- The formal HTML(5) specification: [https://html.spec.whatwg.org/multipage/](https://html.spec.whatwg.org/multipage/)
- [https://www.w3schools.com/](https://www.w3schools.com/)

Note that, again, if the source material for an EPUB production is an EPUB file, or any other HTML or XML based material, it can not be assumed that the mark-up of the contents is already correct. Proper analysis of the content and its semantics must always be performed by the Supplier in accordance with this document.

### Pagination

All page breaks occurring in the source copy are required to be indicated with one of the following elements unless stated otherwise by the Ordering Agency:

- Inline: `<span epub:type="pagebreak" role="doc-pagebreak">`
- Block: `<div epub:type="pagebreak" role="doc-pagebreak">`

In general, block page break markers are used when the page break occurs between two block level elements. Lists are an exception. If a page break occurs between list items, an inline marker is placed at the end of the last item on the page. If a page break occurs in a table, an inline marker is placed in the last cell of the last row on the page.

Additionally required attributes:

- `aria-label=""`
- `class=""`
- `id=""`

The value for the `aria-label` attribute must be identical to the page number in the source copy. For empty pages occurring for example between chapters, this attribute must have a value corresponding to the number implicit for that page.

In those cases where pagination of a text cannot be effectively represented using the following rules, the Supplier is required to contact the Ordering Agency.

The class attribute must have either of the following values:

- `page-front`
- `page-normal`
- `page-special`

The attribute value `page-normal` is required for the pagination of the main contents of the book, basically the bodymatter, but can also be used for the frontmatter and backmatter if the pagination is continuous.

The attribute value `page-front` is required when the page numbering series of the frontmatter is not logically continued in the publication bodymatter, i.e. roman numerals.

The attribute value `page-special` is required for any parts of the book not numbered in a standard manner, for example appendices, suites of photography etc.

The attribute `id` is simply a unique identifier.

By default, the pagebreak elements are required to be empty, like in the following examples:

- Inline: `<span epub:type="pagebreak" role="doc-pagebreak" class="page-normal" id="page-38" aria-label="38"></span>`
- Block: `<div epub:type="pagebreak" role="doc-pagebreak" class="page-normal" id="page-38" aria-label="38"></div>`

However, there may be agency specific instructions to place the page number, as it is displayed in the source material, as content in the pagebreak elements. For example:

```html
<span epub:type="pagebreak" role="doc-pagebreak" class="page-normal" id="page-38" aria-label="38">38</span>`
```

This is only to be done if specific instructions are given by the Ordering Agency.

There must be no unnumbered pages. If unnumbered pages in the source material are implicitly numbered, for instance the initial pages of a book, they must be numbered accordingly. That is, if the pagination starts with page 9 in the source material, the previous pages must be numbered 1-8. If there are other unnumbered pages in the source material they must be assigned numbers and given the `class` attribute `page-special`. This will be specified by the Ordering Agency via Editing Instructions.

### Headings

The `<h1>-<h6>` elements are used to reflect the heading structure present in the source copy. Note that `<h[x]>` tag must be contained within its respective sectioning element. Sectioning elements that may require headings are, for instance, `<section>`, `<aside>` and `<nav>`.

Some chapter or part headings consist of a main heading and a subtitle. For subtitles, use a `<p>` tag with `epub:type="subtitle"` and `role="doc-subtitle"`. Group the main heading and subtitle in an `<hgroup>` element.

```html
<hgroup>
    <h1>Main heading</h1>
    <p epub:type="subtitle" role="doc-subtitle">Subtitle</p>
</hgroup>
```

Note that the subtitle should be included in the section's navigation TOC entry. See section [EPUB 3.2 Navigation Document](#epub-32-navigation-document) for detailed instructions.

Sometimes the source material has a chapter or part number, or a phrase indicating the number, followed by a chapter or part heading as two distinct block elements. In this case, the number is included in the same `h[x]` element as the heading that follows it. The number is marked up with a `<span>` with `class="chnum"` or `class="partnum"`. A `<br/>` tag is inserted after the `<span>`.

```html
<h1><span class="chnum">Chapter 1</span><br/>Chapter heading</h1>
```

Headings that do not contribute to the hierarchical structure of the work and that are not desired to be included in the navigation document can be given the attribute `class="no-toc"` and then, consequently, be omitted from the navigation document. This mark-up may be specifically requested by the Ordering Agency via Editing Instructions and must never be used otherwise.

In some cases heading mark-up may not be desired at all, for usability reasons. In these cases, `<p class="faux-hd">` may be used, but only if specific instructions are given by the Ordering Agency.

#### Continuation Headings

Consider the following example: 

```html
<section>
    <h[x]>Section heading</h[x]>
    <p>...</p>
    ...
    <section>
        <h[x+1]>Sub-section heading</h[x+1]>
        ...
    </section>
    <p>Content belonging to the section on level x.</p>
    ...    
</section>
```

Here, there is more content belonging to the outer `<section>` element after the closing of the sub-section. In cases like this, there may be a need for a continuation heading, that tells the user that text belonging to the `<h[x]>` heading will continue. This heading will have the same level as the initial heading of the section, but must  also have the class attributes  `no-toc` and `cont-hd`. The contents of the heading  must be the same as the initial heading with the text "(continued)" added. Note that this addition is language dependent  and language-specific terms can be found in section 5.2.4. The example above would then be:

```html
<section>
    <h[x]>Section heading</h[x]>
    <p>...</p>
    ...
    <section>
        <h[x+1]>Sub-section heading</h[x+1]>
        ...
    </section>
    <h[x] class="no-toc cont-hd">Section heading (continued)</h[x]>    
    <p>Content belonging to the section on level x.</p>
    ...    
</section>
```

Continuation headings must only be used if specific instructions are given by the Ordering Agency.

#### Chapter authors

Sometimes an author name appears before or after the chapter heading. Use the markup `<p class="chapter-author">` for a paragraph containing one or more author names. Note that this markup should not be used for author names at the end of a section.

### Figures

All proper figures, illustrations, photographs, icons and other symbols must be captured as images and stored in the EPUB file, unless other instructions are given (see section 2.7). Purely decorative graphics that have no other purpose than layout can be ignored. If there are any doubts about whether to include certain graphics or not, the Supplier is required to contact the Ordering Agency.

Unless the image occurs inline in the source material, any image is required to be placed inside a `<figure>` element with a `class` attribute set to `image`. If the image has a caption, the caption is required to be marked up with the `<figcaption>` element and placed as either the first or the last child of the `<figure>` element.

Small symbols or other non-typographical content that might occur inline, are required to be represented with `<img>` elements alone, without the `<figure>` container. There may be cases where it is unclear whether to regard symbols or icons as inline or not, for instance small icons in connection with exercises. Specific instructions on how to handle these may be given in Editing Instructions. If doubt remains, the Supplier is required to handle the image as a block element and use `<figure>` or, alternatively, contact the Ordering Agency.

Images in tables or lists may be handled as inline images if there are no captions or similar.

#### Alt-texts

Accessibility guidelines require images to be supplied with a short, descriptive text as value of the `alt` attribute of the `<img>` element. Suppliers are not required to provide these descriptive texts. If the source material is a publisher file that includes alt-texts, these must, however, be preserved. For images that do not already have an alt-text, the supplier should use one of the following generic values:

- `Photo.` – for photographs
- `Illustration.` – for illustrations
- `Figure.` – for schematic images, graphs, diagrams, scientific models etc.
- `Symbol.` – for icons, signs, inline images etc.
- `Map.` – for maps
- `Drawing.` – for drawings
- `Comic.` – for comic strips and panels
- `Logo.` – for logos

Section 5.2.3 of this document contains language specific alt-text values.

If there are any doubts about which value to assign the `alt` attribute, suppliers are required to use `figure`.

<div class="note">

The list of generic categories for Supplier to apply as alt-text values do not alone satisfy [WCAG's success criterion 1.1.1 Non-text Content](https://www.w3.org/TR/WCAG/#non-text-content), as they are in most cases not sufficient textual alternatives to the images. The guidelines here presume post-markup editing to make productions fully WCAG-compliant, by editing the alt text values and/or adding extended image descriptions.

</div>

<div class="note">
For alt-texts provided by the Supplier to be properly read by screen-readers they should always end with a punctuation mark.
</div>

#### Text Extraction from Images

When images contain text that is integral to the image itself, i.e. not a caption or similar, this text is required to be extracted as accessible text. This text must be placed in a placeholder element, either within the `<figure>` element of the image or directly after it. The placeholder elements can be:

- `<aside>` This is the default option. It can be placed inside the `<figure>` element or directly after it. If it is placed inside it must be placed after the `<img>` element, before the closing `</figure>` tag. If there is also a `<figcaption>`, this must be placed before the `<img>` element. If the `<aside>` is placed after the closing `</figure>` tag, there is no restriction on the placement of the `<figcaption>`. It is up to the Ordering Agency to specify which option to be used, either in Agency-specific Guidelines or in Editing Instructions.
- `<details>` This element must only be used if specific instructions are given by the Ordering Agency. It is required to be placed directly after the `<figure>` element, never inside it.

The placeholder  element is required to have the following attributes:

- `class="fig-desc"`
- `id=""`

where `id` is a unique identifier. Furthermore, the corresponding `<img>` element must then be given the following attribute:

- `aria-describedby=""` if the placeholder is `<aside>`
- `aria-details=""` if the placeholder is `<details>`

with the same value as the id of the `<aside>` placeholder.

The extracted text is then placed inside the placeholder, marked up correctly and placed in a logical reading order, if there is any. Here is an example of how a figure with extracted text can be handled using the `<aside>` option:

```html
<figure class="image">
	<figcaption>...</figcaption>
	<img src="images/X41001A-012.jpg" alt="figure" aria-describedby="desc012" />
	<aside class="fig-desc" id="desc012">
		<p>...</p>

	</aside>
</figure>
```

Note that this is the construction that is used by several Ordering Agencies for adding image descriptions in post-production.

If an inline image require text extraction the extracted text must be used as value for the `alt` attribute. The text then replaces the generic value described above.

#### Image Series

If the source material contains a series of images, images that are linked in some way, each individual image must be marked up as described above with each image wrapped in separate `<figure>` elements. The whole series must then be wrapped in a `<figure>` element where the `class` attribute is set to `image-series`.

If there is a caption for the image series as a whole, this must be marked up with the `<figcaption>` element and placed inside the outer `<figure>` element, before the individual `<figure>` elements containing the images. Any caption for individual images are placed together with that image as described above.

This is how the markup will look like:

```html
<figure class="image-series">
   <figcaption>...</figcaption>
   <figure class="image">
      <figcaption>...</figcaption>
      <img src="images/X41001A-012.jpg" alt="figure" aria-describedby="desc012" />
      <aside class="fig-desc" id="desc012">
         <p>...</p>
      </aside>
   </figure>
   <figure class="image">
      ...
   </figure>
   ...
</figure>
```

### Lists

Lists are a number of connected items (single words, sentences or whole paragraphs) written consecutively. They can be numbered or unnumbered. Any such content is required to be marked up with either `<ol>` (ordered list) or `<ul>` (unordered list). Each item of any list must be marked up with `<li>`.

A list item may either contain inline content or block elements, but not a mixture of both. As a rule of thumb, if all items in the list consist of single words or short phrases no further block elements are needed. If one or more of the list items consist of sentences or paragraphs, use one or more `<p>` elements inside every list item of the list.

#### Numbered Lists

The numbering of an ordered list must not be included as content in the `<li>` elements of the list. The numbering will be rendered by the reading system. The default type for the numbering is numeric. This can be changed by using the `type` attribute. The default starting point is `1` (regardless of which type of numbering the ordered list uses), but can be changed using the `start` attribute.

#### Unnumbered Lists

Unnumbered lists often have some sort of bullet markers for each list item. The default is a solid black circle. The type attribute has been used before, to change the type of bullet symbol, but this is not supported in HTML 5. Using CSS is the proposed method of controlling this. Suppliers are not required to modify the CSS file to match the bullet markers of the source material unless specifically instructed to do so.

Lists without any bullet markers are required to have the attribute:

- `class="plain"`

By default, `<ul>` should be used here, but Ordering Agencies may give specific instructions to use `<ol>`.

#### Tables of Contents

Any table of contents in the source material is required to be marked up in the following way:

```html
<ol class="plain" epub:type="toc" role="doc-toc">
   <li><span class="lic">_[Heading 1]_</span> <span class="lic">_[Page number]_</span></li>
   ...
</ol>
```

This is typically used for the main table of contents that most books have somewhere in the frontmatter part, but it could also be a table of content for a single part or chapter. The `<span class="lic">` element, used to separate the headings from the page references, must not be used in any other context. Any line that does not have a page reference must be marked up using only `<li>`, without any `<span class="lic">`.

Sometimes, mostly in educational books, the table of contents can be more complicated, including other type of content than simply headings and page references. In these cases, specific instructions will be given by the Ordering Agency of how to handle that. Contact the Ordering Agency if anything is unclear.

### Tables

All tables or table-like structures are required to be marked up as `<table>`. If the table has a caption it is required to be marked up with `<caption>` and placed just after the starting tag of the `<table>` element. It can sometimes be unclear what content should go into the `<caption>` element. Sometimes there is a title in the table itself, spanning the entire width. This must be removed from the table structure and placed in the `<caption>` element. Sometimes there could be a regular caption above the table and a source reference at the bottom. These must both go into the `<caption>` element in individual paragraphs. Non-standard use of the `<caption>` element should be specified by the Ordering Agency in the Editing Instructions.

The `<tbody>` element is required to be used for containing the main body of table data. It is recommended, although not formally required, to use `<thead>` for any column heading at the top of the table. For the sake of consistency, it is required to use `<thead>` if there is at least one row of column heading at the top of the table, unless specific instructions are given to omit it. The element `<tfoot>` can be used if there are, for instance, a row at the bottom of the table where columns are summed up. This is not required, but can be included in Editing Instructions. Note, that `<tbody>` can be used multiple times in the same table, if the table is divided into sections. Usually there will also be a section heading for each section. See the code example at the end of this section.

Each row of the table, with all its table cells, must be placed inside a `<tr>` element and each individual table cell must be placed inside either a `<td>` element for regular table data, or inside a `<th>` element for column or row headings. Headings are typically found in the top-most row and the left-most column of the table, but there may be heading cells elsewhere and there may be headings for groups of rows or columns. The `<th>` element is required to have the attribute `scope` with  a value that specifies what the header is for in the following cases:

- when heading cells are merged with colspan or rowspan, or
- when column headings are present anywhere else than in the first (topmost) row, or row headings are present anywhere else than in the first (leftmost) column.

The possible values are `col` or `row`, for column and row, respectively, and `colgroup` and `rowgroup`, if the table has headings spanning multiple columns or rows with individual sub-headings. If none of the criteria mentioned above is fulfilled, `scope` is not required. 

Tables are required to have a consistent number of table cells per row. If the `colspan` or `rowspan` attributes are used, take extra care that the total number of cells is correct. 

Never use tables solely for the purpose of mimicking the layout of the source material.  The `colspan` and `rowspan` attributes may be used with `<td>` or `<th>` elements, if necessary, but if the purpose of the layout in the source material is unclear and no instructions are given, Suppliers are required to contact the Ordering Agency for clarification.

Below is an example of table mark-up that covers most of the details explained above.

```html
<table>
    <caption>Table 1.1. Full example.</caption>
    <thead>
        <tr>
            <th></th>
            <th>Column 1</th>
            <th>Column 2</th>
            <th>Column 3</th>
        </tr>            
    </thead>
    <tbody>
        <tr>
            <th colspan="4" scope="rowgroup">1990</th>
        </tr>
        <tr>
            <th>Row 1</th>
            <td>A</td>
            <td>B</td>
            <td>C</td>
        </tr>            
        <tr>
            <th>Row 2</th>
            <td>D</td>
            <td>E</td>
            <td>F</td>
        </tr>            
    </tbody>
    <tbody>
        <tr>
            <th colspan="4" scope="rowgroup">2020</th>
        </tr>
        <tr>
            <th>Row 1</th>
            <td>G</td>
            <td>H</td>
            <td>I</td>
        </tr>            
        <tr>
            <th>Row 2</th>
            <td>J</td>
            <td>K</td>
            <td>L</td>
        </tr>            
    </tbody>
</table>    
```

Large tables may give rise to visual display issues where content overflows the page area. One way to handle this is to add scroll bars to the table through CSS. For this to be possible, the table needs to be wrapped in a `div` with `class="table-wrapper"`. The Ordering Agency will specify in Editing Instructions if this optional markup is needed.

##### Table id:s for lot
In cases where a title contains a list of tables (`lot`) or where a list of tables should be included in the `nav.xhtml`-file, the table element  should be provided with an id. These id:s should be named `"tab-XXX"` in sequential order, where XXX is the number of the table in the file.
Instances may occur where id:s are required for tables without it being included in the `nav.xhtml`-file.

### Definition Lists

All paired lists of words, phrases, expressions etc. and corresponding definitions, translations etc. are required to be marked up as `<dl>`. Note that language attributes may be required, for example with glossaries.

### Notes and Note References

#### Note References

Note references are required to be marked up as hyperlinks with the `role` attribute set to `doc-noteref` and `epub:type` set to `noteref`. The `<a>` element must also have a unique `id` attribute. Note references are not to be marked up with `<sup>` as styling will be handled by a default CSS file provided by the Ordering Agency.

#### End notes

End notes or chapter notes must be placed in a `<section>` element at the end of the content file, before the closing of the top-level `<section>` element. The `<section>` element containing the notes is required to have the `role` attribute set to `doc-endnotes` and `epub:type` set to `endnotes`. If the source material has a section like this and it has a heading, this heading is required to be preserved and marked up as `<h2>`. If there is not a heading for the note section present in the source, it is required to be added. Section 5.2.5 of this document contains language specific headings to be used.

If the notes have sequential numbers, the note texts are required to be marked up as an ordered list. The list items containing the note texts must have unique `id` attributes that the note references can link to. Note that the note numbers will be automatically generated by the `<ol>` markup and must not be included in the text. 

```html
<section role="doc-endnotes" epub:type="endnotes">
   <h2>Notes</h2>
   <ol>
      <li id="fn1">
         <p>...</p>
      </li>
      <li id="fn2">
         <p>...</p>
      </li>
      ...
   </ol>
</section>   
```

If, for some reason, an ordered list can't be used, each note text is required to be placed in a separate `<div>` element. The `<div>` elements must have unique `id` attributes that the note references can link to. In this case the number or other identifier of the note text must be included in the text.

```html
<section role="doc-endnotes" epub:type="endnotes">
   <h2>End notes</h2>
   <div id="fn1">
      <p>...</p>
   </div>
   <div id="fn2">
      <p>...</p>
   </div>
   ...
</section>   
```

#### Footnotes

Footnotes that are sequentially numbered are required to be handled as end notes (see the section above). The note texts must be placed at the end of the content file, in its own `<section>` element and all `role` and `epub:type` attributes must be set as end notes. This goes for the note references as well.

Other types of footnotes are to be placed in an `<aside>` at the end of the relevant `<section>` or before the next `<section>`, whichever is the nearest to the note reference. These would typically be notes referenced by an asterisk or some other symbol. If there are more than one note reference in the section, notes must **not** be grouped in a single `<aside>`, but separated so that each note is contained in its own `<aside>`. The hyperlink of the note reference is required to point to the `id` attribute of the `<aside>` element containing the note text. The `<aside>` element is required to have the `role` set to `doc-footnote` and `epub:type` set to `footnote`. 

```html
<p>... <a id="ref1" href="#fn1" role="doc-noteref" epub:type="noteref">*</a> ...</p>
...
<aside id="fn1" role="doc-footnote" epub:type="footnote">
   <p>* ...</p>
</aside>
```

If notes occur in tables, the note texts are required to be handled as footnotes and must not be placed in a `<tfoot>` element within the `<table>` structure. They must be placed directly after the table.

#### Backlinks

Backlinks are required for each note, pointing back to the note reference. The `<a>` element is required to have `role` set to `doc-backlink`.  For both footnotes and endnotes, the backlink is required to be placed in its own paragraph, after the note text. The backlink text must contain the number or identifier of the note.

```html
<ol>
   <li id="fn1">
      <p>...</p>   
      <p><a href="#ref1" role="doc-backlink">Go to note reference 1.</a></p>
   </li>
   ...
</ol>   
```

Section 5.2.6 of this document contains language specific backlink texts. Unless other instructions are given by the Ordering Agency, these texts are to be used, based on the language of the publication. Note that the # character must be replaced by the number or other identifier for the note in question.

If there are more than one note reference for a single note text, the note text must be repeated for each note reference so that there is a one-to-one relationship between note references and note texts and all backlinks are unique. This may cause one or more numbers to be repeated and, if so, note texts must be marked up using `<div>` elements instead of and ordered list.

### Sidebars, Text Boxes etc.

The `<aside>` element is required to be used for any material that is placed in the margin, breaks the flow of the main text or is in some other way to be considered optional or non-essential.

Any material that is in some way contained or has a clearly visible beginning and end, but is still an integral part of the main text or content is required to be marked up with `<div>`. This could be, for instance, examples, definitions, tips, boxes containing words or phrases in exercises etc.

In both cases the elements are required to have a `class` attribute. The default value of the class attribute is text-box, but other values can be given in Editing Instructions.

For certain publications there may be instructions given to add `aria-label` attributes to either of the elements, but unless such instructions are given no `aria-label` attributes are to be included.

If there are any doubts about which element to use and there are no further instructions available in the Editing Instructions, Suppliers are required to contact the Ordering Agency.

If headings in text boxes can be seen as part of the hierarchical heading structure of the book, they should be marked up using a `<h[x]>` element of an appropriate level, following the level structure of the surrounding text, and should be included in the navigation document.

Structurally insignificant headings should be marked up with `<h[x]>` with the attribute `class="no-toc"`, or, for instance if they are often repeated, marked up as `<p class="faux-hd">`. Both of these options must only be used if specific instructions are given by the Ordering Agency.

<div class="note">
At the time of publication, the EPUB accessibility validation tool [Ace by DAISY](https://daisy.github.io/ace/) raises moderate best practice violations for multiple unlabelled `<aside>` elements in the same content file. This is expected to change, as available tools and best practices are updated to reflect [the mapping of the HTML aside element to accessibility APIs](https://www.w3.org/TR/html-aam-1.0/#el-aside), which considers the aside element an WAI-ARIA landmark (complementary), only when it has an accessible name. When triggered by non-labelled aside elements, Suppliers can safely ignore the violation presented by Ace by DAISY.
</div>

#### Nested text boxes
In some cases, books contain text boxes containing other text boxes. In those cases, Suppliers should generally only use one level of `<aside>` elements, e.g. not nest two or more `<aside>`s. Instead, `<div class="text-box">` can be used for the inner level box (see above). Another possibility is to consider whether the parent text box could actually be a `<div>`, and then the inner level box an `<aside>`.

### Computer Code

Suppliers are required to mark up code content with the `<code>` element. For block instances containing several lines of code, the `<code>` element must be contained in a `<pre>` element.

In blocks of computer code, spaces, line breaks and empty lines must be preserved. It should be noted, however, that the correct markup for code blocks involves interpretation alongside the principle of capturing the text verbatim. For example, programming books may sometimes use a mix of semantic line breaks, which must be preserved, and line breaks necessitated by the print layout.

### Bolding and Italics

The only elements to be used are `<strong>` for bold and `<em>` for italics. Other type of formatting may be required in certain publications and, if such is the case, specific instructions will be given in the Editing Instructions.

Principles for how `<strong>` and `<em>` are used can be found here:

[http://kb.daisy.org/publishing/docs/html/emphasis.html](http://kb.daisy.org/publishing/docs/html/emphasis.html)

Please take care that text marked up with em or strong is identical with the original book. Do not include space or punctuation which is not emphasised in the original. Words in em or strong at the end of sentences should not include end-of-sentence punctuation, like full stops, unless the whole sentence is emphasised. If a sentence ends with an emphasised word, and the next sentence begins with a new emphasised word, make sure the words are marked up using separate instances of em or strong, and that the end-of-sentence punctuation is not included.

### Poetry and Verse

Poetry, song lyrics or any content written in verse, where lines of text must be preserved just as they are in the source material, is required to be marked up with `<div class="verse">`.

Each group of lines of text must be contained in a separate `<p class="linegroup">` element and each line of text must be marked up with `<span class="line">`. HTML line breaks, `<br/>`, must be added between consecutive lines within a line group. Indented lines may be marked up using `<span class="line_indent">` or `<span class="line_longindent">`.

Line numbers must only be included if specific instructions are given about it, even if they are present in the source material. If line numbers are to be included, they must be marked up with `<span class="linenum">`.

If the content written in verse has a title it may be handled as a normal heading, with `<section class="verse">` wrapping the content. Note that `class="verse"` must be used here as well. However, if it does not make sense to use a proper heading, it may be marked up with `<p class="faux-hd">` and placed within the `<div class="verse">` container. This option must not be used unless specific instructions are given by the Ordering Agency.

If there is an author name placed under the verse it may be marked up with `<p class="verse-author">` and placed at the end of the `<div class="verse">` container.

In some cases, where poetry has figurative or otherwise non-standard disposition of the text, it may be requested by the Ordering Agency to also capture the verse as an image. It may also be requested that the verse is captured as pre-formatted text, using the `<pre>` element. Neither of these options must be used unless specific instructions are given by the Ordering Agency. 

#### Use of line/linenum-formatted content in non-verse content. 

In some titles, line-by-line based markup will be needed where `<div class="verse">` is not applicable (e.g. easy-to-read book content, latin original texts, verbatim reproductions of text for analysis in methodology literature). For these titles `<div class="line-by-line>` should be used instead. 

In the cases where `<div class="line-by-line>` are used, the same rules as for `<div class="verse">` are applied. Any variations to the markup in `<div class="line-by-line>` are given by the Ordering Agency.

### Quotes

Quotes, citations, excerpts from other sources and similar content are required to be marked up using the `<blockquote>` element whenever the content is separated from the regular text. Often, this type of content is distinguished from the regular text via indentation or some type of different styling. The `<blockquote>` element is required to wrap everything that connects to the quote or citation. For instance, if there is a source reference underneath the quote itself, this must also be included in the `<blockquote>` element, marked up with a separate `<p>`.

### Hyperlinks and URLs

Any URLs in printed source material are required to be represented as plain text in the EPUB file and not be made into an active hyperlink. In digital sources, already active hyperlinks are required to be preserved, but an URL that is just plain text is required to remain as such.  

### Numbered paragraphs

Some books, such as editions of classical literary works, may have numbered paragraphs or stanzas in the source material. Unless the ordering agency gives instructions to exclude the numbering, the supplier is required to preserve it. The numbers are placed at the beginning of the paragraph and marked up with `<span class="parnum">`. The paragraph is given `class="numbered"`.

Example 1, prose:

```html
<p class="numbered"><span class="parnum">1</span>Paragraph text.</p>
```

Example 2, poetry and verse:

```html
<p class="linegroup numbered">
    <span class="parnum">1</span><span class="line">First line.</span><br/>
    <span class="line">Second line.</span>
</p>
```

### Thematic Breaks in the Text Flow

In circumstances where depth of structure is not amenable to structural markup using `<section>` elements, Suppliers are required to use the `<hr>` element to provide distinguishable paragraph-level thematic breaks in one of the following two ways:

- `<hr class="emptyline"/>` indicates thematic breaks represented by a vertical space between paragraphs.
- `<hr class="separator"/>` indicates thematic breaks represented by a visual marker such as an asterisk, horizontal rule or any other type of graphical symbol. The visual marker must not be rendered as content.

### Language Tagging

Content documents may contain text in other languages than the main language, as defined in the root element (see section [Language Definition](#language-definition)). For longer passages comprising one or more block elements, the language must be specified using the `lang` and `xml:lang` attributes. Elements that may require these attributes include `<p>`, `<ol>`, `<ul>`, `<blockquote>`, `<aside>` and `<section>`. Inline text is not marked up unless specifically indicated by the Ordering Agency.

Please note that the level of language markup required by suppliers in these guidelines may not in all cases meet the [WCAG success criterion 3.1.2 Language of Parts](https://www.w3.org/TR/WCAG/#language-of-parts).

### Uppercase text

As a general rule, all text in the XHTML files should be written in sentence case: The first word of the sentence or heading is capitalised, as well as proper nouns and other words as required by a more specific rule. Some acronyms are written in all uppercase. If all caps (or small caps) formatting is desired in cases where the use of capital letters is not required by ortography, it should taken care of through CSS.

In many cases, however, the source material contains text styled as all caps or small caps, which is transferred to the XHTML files as uppercase letters. This is commonly found in headings and in lead-ins, where the first words of a chapter or block of text are set in all caps or small caps in the source.

In general, the Supplier is not expected to normalise capitalisation (change it to sentence case) unless the Ordering Agency specifically requests this.

For lead-ins, the Supplier must normalise capitalisation and apply `<span class="lead-in">` to the lead-in text. In doing this, care should be taken to respect capitalisation patterns in the surrounding text as well as language specific conventions.

All caps or small caps may also be used to emphasise words or short passages of body text. If the source file is an EPUB or other HTML based publication where such styling is applied using CSS, the markup `<strong class="all-caps">` or `<strong class="small-caps">` should be used to preserve the styling.

### Styling

In general, styling and layout properties must be ignored. The styling and layout of the EPUB file will be handled by standard CSS files provided by the the Ordering Agencies. However, there are cases when styling used in the source material carries a meaning. In these cases styling classes may be required. Typical examples of this can be:

- Words, phrases or expressions written in a different colour and being referred to elsewhere in the text
- Text boxes with a certain background colour that explains the function of the box
- Words or phrases written in a different font for some specific reason

Note that these are only examples and other types of significant styling may occur. The `class` attributes must be placed in a `<span>` element wrapping the words or phrases in question, or in an already present element where that is applicable. Only `class` attributes defined by the Ordering Agency may be used. The `style` attribute is not allowed. The Suppliers must only apply styling classes when given specific instructions by the Ordering Agency.

Since the meaning conveyed through styling will not be accessible to all readers of the EPUB file, there may be a need for post production editing to ensure full WCAG compliance.

# Specific Requirements for Advanced Content

## Numbers and STEM Content

Numbers are required to be captured exactly as they are in the source material. Decimal and thousand separators must be preserved as they are, using the exact same characters. If space is used as thousand separator in large numbers, non-breaking space, the character `&#160;`, must be used. Note, that the HTML entity `&nbsp;` is not allowed. If a number has a unit attached to it, the unit should be captured as regular text and any exponents marked up with `<sup>`, unless the number is part of a mathematical expression.

Simple mathematical expressions or other simple STEM related content in non-STEM contexts can be captured as regular text with standard HTML used to mark up exponents or indices. The correct Unicode characters must be used to represent mathematical operators or special characters. Typical examples of this may be:

- Simple arithmetic expressions written in linear form, like 1 + 2 = 3
- Chemical formulae for common substances, like `CO<sub>2</sub>`
- Greek letters

If standard HTML is unable to represent the expression exactly as it looks in the source material, [[MathML3]] must be used. Therefore, more complex expressions are required to be marked up with MathML, even in non-STEM contexts. If it is unclear whether to use MathML or not, and no specific instructions are given, the Supplier must ask the Ordering Agency for clarification. 

In STEM-related materials, all STEM content, excluding stand-alone numbers or single-letter variables, must be marked up using MathML to ensure a uniform presentation of the content to the user.  These guidelines will not go into details about the MathML language. Please refer to the specification.

For more detail about the MathML structure, please refer to each individual Ordering Agency's requirements. Note that some Ordering Agencies may not require MathML at all, at present. ASCIIMath notation may be requested instead or in combination with MathML markup. See [http://asciimath.org](http://asciimath.org) for information about the ASCIIMath notation. 

## Special characters, punctuation, etc.
### Representation of characters
If not requested otherwise by the Ordering Agency, Unicode characters should be represented by the correct Unicode character rather than an entity reference using e.g. decimal or hexadecimal notation. An inverted question mark, used in for example Spanish, should be represented as `¿` rather than `&#191;`, `&#xbf;`, or similar.

Note that entity reference coding of special characters may be requested on a per-production or per-Ordering Agency basis.

### Character accuracy
Unicode character accuracy for special characters, e.g. phonetic characters, is vital for a correct representation of the source material. A visual likeness to the characters used in the printed source material is not enough, as the characters may be used by assistive technology to present text information to the user, or being used in various conversions to other types of end-user formats. If there is a Unicode representation for any character or symbol used in an inline context this should be used, rather than capturing the symbol as an image. Emojis are an example of where it is important to use the Unicode representation instead of capturing them as images.

The following resources could be valuable for verifying commonly used special characters:

- Phonetics: [https://www.phon.ucl.ac.uk/home/wells/ipa-unicode.htm](The International Phonetic Alphabet in Unicode (UCL))
- The Greek alphabet: [https://www.fileformat.info/info/unicode/block/greek_and_coptic/list.htm](Unicode Characters in the Greek and Coptic Block (FileFormat.info))
- Emojis: [https://unicode.org/emoji/charts/full-emoji-list.html]

Similarly to phonetics and other special characters, punctuation, such as quotation marks, dashes, etc., should be preserved as they are represented in the source material. This means that careful attention needs to be payed to ensure correct representation of e.g. hyphen minus (- (U+002D)) vs. en dash (– (U+2013)) vs. em dash (— (U+2014)), hyphen minus vs. mathematical minus sign (− (U+2212)), simple quotation marks ("" (U+0022)) vs. typographic quotation marks (”“ (U+201D, U+201C)), etc. Any exceptions to this general rule will be noted in Editing Instructions.

### Ligatures
Depending on the typography of the source material, ligatures may be present in source text. These may **not** be captured as dedicated ligature unicode characters, e.g. "ﬆ" (U+FB06). Rather, ligatures should be normalised to their separate letter components (e.g. "st") upon capture.

## Placeholders for User Input Areas

In educational material, especially for younger children, it is common that the user is supposed to answer questions or solve problems by writing directly in the printed book.  Usually, this is indicated by printed lines where the user can write text or boxes that can be ticked etc. Suppliers are required to use the `<span>` element to provide placeholders for these input fields in one of the following three ways:

- `<span class="answer">---</span>` for a horizontal line or box where any amount of text or numbers are meant to be inserted. Multiple lines in succession must be represented by a single `<span class="answer">---</span>` unless other instructions are given by the Ordering Agency.
- `<span class="answer_1">-</span>` for a single space where only one character is meant to be inserted, typically a missing letter in a word or similar. If there are two missing letters in a word, there must be two `<span class="answer_1">-</span>` elements, without space between them.
- `<span class="box">---</span>` for check boxes.  

# Appendix

## External Resources

- EPUB 3.2 Specification: [https://www.w3.org/publishing/epub3/epub-overview.html](https://www.w3.org/publishing/epub3/epub-overview.html)
- EPUB Accessibility 1.0 Specification: [https://www.w3.org/Submission/epub-a11y/](https://www.w3.org/Submission/epub-a11y/)
- DAISY Accessible Publishing Knowledge Base: [https://kb.daisy.org/publishing/docs/](https://kb.daisy.org/publishing/docs/)
- HTML(5) Specification: [https://html.spec.whatwg.org/multipage/](https://html.spec.whatwg.org/multipage/)
- Digital Publishing WAI-ARIA Module 1.1: [https://www.w3.org/TR/dpub-aria-1.1/](https://www.w3.org/TR/dpub-aria-1.1/)
- Ace by DAISY Accessibility Checker for EPUB 3: [https://daisy.github.io/ace/](https://daisy.github.io/ace/)
- MARC Code List for Relators Scheme: [https://id.loc.gov/vocabulary/relators.html](https://id.loc.gov/vocabulary/relators.html)
- IANA registry of valid language codes:[https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry)
- EPUB 2 .ncx Legacy Navigation Document Specification: [http://idpf.org/epub/20/spec/OPF_2.0.1_draft.htm#Section2.4.1](http://idpf.org/epub/20/spec/OPF_2.0.1_draft.htm#Section2.4.1)
- MathML Specification: [https://www.w3.org/TR/MathML/](https://www.w3.org/TR/MathML/)
- ASCIIMath Specification: [http://asciimath.org](http://asciimath.org)
- International Phonetic Alphabet in Unicode Reference: [https://www.phon.ucl.ac.uk/home/wells/ipa-unicode.htm](https://www.phon.ucl.ac.uk/home/wells/ipa-unicode.htm)
- Greek and Coptic Letters in Unicode Reference: [https://www.fileformat.info/info/unicode/block/greek_and_coptic/list.htm](https://www.fileformat.info/info/unicode/block/greek_and_coptic/list.htm)

## Language Dependent Fixed Text Values

The default text values given in this document are, for practical reasons, only given in English. Unless otherwise instructed by the Ordering Agency, these values, when applied to a specific publication, should follow the primary language of that publication.

For convenience, the values for all the main languages of the Ordering Agencies are listed in the tables below. The Ordering Agencies will provide Suppliers with canonical translations if the main language of a title is not one of those listed here.

### nav.xhtml Headings

| English (default) | Swedish      | Norwegian (Bokmål) | Norwegian (Nynorsk) | Finnish       | Dutch            | Danish           | Icelandic   | German              |
|-------------------|--------------|--------------------|---------------------|---------------|------------------|------------------|-------------|---------------------|
| Pages             | Sidindelning | Liste over sider   | Liste over sider    | Sivut         | Paginering       | Liste over sider | Blaðsíður   | Seiten              |
| Contents          | Innehåll     | Innhold            | Innhald             | Sisällys      | Inhoud           | Indhold          | Efni        | Inhalt              |
| Landmarks         | Navigation   | Navigasjon         | Navigasjon          | Kiintopisteet | Oriëntatiepunten | Navigation       | Leiðarmerki | Orientierungspunkte |


### aria-label and TOC Values

| English (default)     | Swedish            | Norwegian (Bokmål) | Norwegian (Nynorsk) | Finnish        | Dutch               | Danish                | Icelandic        | German               |
|-----------------------|--------------------|--------------------|---------------------|----------------|---------------------|-----------------------|------------------|----------------------|
| Cover                 | Omslag             | Omslag             | Omslag              | Kansi          | Omslag              | Omslag                | Kápa             | Umschlag             |
| Front cover           | Framsida           | Forside            | Framside            | Etukansi       | Voorkant            | Forside               | Forsíða          | Buchvorderseite      |
| Back cover            | Baksida            | Bakside            | Bakside             | Takakansi      | Achterkant          | Bagside               | Bakhlið          | Buchrückseite        |
| Jacket left flap      | Vänsterflik        | Venstre innbrett   | Venstre innbrett    | Etulieve       | Linker flaptekst    | Venstre inderflap     | Vinstra innábrot | Vorderer Klappentext |
| Jacket right flap     | Högerflik          | Høyre innbrett     | Høgre innbrett      | Takalieve      | Rechter flaptekst   | Højre inderflap       | Hægra innábrot   | Hinterer Klappentext |
| Title page            | Titelsida          | [to be added]      | [to be added]       | Nimiö          | [to be added]       | Titelside             | [to be added]    | Titelseite           |
| Publisher information | Förlagsinformation | Utgiverinformasjon | Utgivarinformasjon  | Julkaisutiedot | Uitgeversinformatie | Kolofon               | Útgefandi        | Verlagsangaben       |
| Dedication            | Dedikation         | Dedikasjon         | Dedikasjon          | Omistus        | Opdracht            | Dedikation            | Tileinkun        | Widmung              |
| Epigraph              | Epigraf            | Epigraf            | Epigraf             | Epigrafi       | Epigraaf            | Epigraf               | Tilvitnun        | Epigraf              |
| Contents              | Innehåll           | Innhold            | Innhald             | Sisällys       | Inhoud              | Indhold               | Efni             | Inhalt               |
| Start of Content      | Innehållets början | Start av innhold   | Start av innhald    | Sisällön alku  | Begin inhoud        | Indholdets begyndelse | Byrjun           | Beginn des Inhalts   |


### Image Alternative Text Values

| English (default) | Swedish        | Norwegian     | Finnish    | Dutch         | Danish        | Icelandic  | German        |
| ----------------- | -------------- | ------------- | ---------- | ------------- | ------------- | ---------- | ------------- |
| Photo.            | Foto.          | Foto.         | Valokuva.  | Foto.         | Foto.         | Ljósmynd.  | Foto.         |
| Illustration.     | Illustration.  | Illustrasjon. | Kuvitus.   | Illustratie.  | Illustration. | Mynd.      | Illustration. |
| Figure.           | Figur.         | Figur.        | Kuvio.     | Figuur.       | Figur.        | Myndefni.  | Figur.        |
| Symbol.           | Symbol.        | Symbol.       | Symboli.   | Symbool.      | Symbol.       | Tákn.      | Symbol.       |
| Map.              | Karta.         | Kart.         | Kartta.    | Kaart.        | Kort.         | Kort.      | Karte.        |
| Drawing.          | Teckning.      | Tegning.      | Piirros.   | Tekening.     | Tegning.      | Teikning.  | Zeichnung.    |
| Comic.            | Tecknad serie. | Tegneserie.   | Sarjakuva. | Stripverhaal. | Tegneserie.   | Myndasaga. | Comic.        |
| Logo.             | Logotyp.       | Logo.         | Logo.      | Logo.         | Logo.         | Lógó.      | Logo.         |

### Continuation Heading Identifiers

| English (default) | Swedish        | Norwegian     | Finnish       | Dutch         | Danish        | Icelandic     | German        |
|-------------------|----------------|---------------|---------------|---------------|---------------|---------------|---------------|
| (continued)       | (fortsättning) | [to be added] | (jatkuu)      | [to be added] | [to be added] | [to be added] | [to be added] |

### End Note Heading

| English (default) | Swedish  | Norwegian     | Finnish       | Dutch         | Danish        | Icelandic     | German        |
|-------------------|----------|---------------|---------------|---------------|---------------|---------------|---------------|
| Notes             | Noter    | [to be added] | Viitteet      | [to be added] | [to be added] | [to be added] | [to be added] |

### Footnote and Endnote Backlink

| English (default)       | Swedish                         | Norwegian              | Finnish               | Dutch                     | Danish                  | Icelandic           | German               |
|-------------------------|---------------------------------|------------------------|-----------------------|---------------------------|-------------------------|---------------------|----------------------|
| Go to note reference #. | Gå tillbaka till notreferens #. | Gå til notereferans #. | Siirry viitteeseen #. | Ga naar nootreferentie #. | Gå til notereference #. | Aftur í tilvísun #. | Gehe zur Referenz #. |

## Additional valid markup
In addition to the features documented above, the Nordic Guidelines recognise some additional markup constructions that are allowed in valid 2024-1 files.

Suppliers do not need to observe these unless specifically requested by Ordering Agencies, as they will mostly be used in post production at the Ordering Agencies. They are listed here primarily for documentational purposes.

### Sentence markup
Sentence markup using `<span>` and `@class="sentence"` is allowed. This is used in Media Overlays talking book production, as targets for SMIL references when providing sentence-level synchronised audio.

<section class="appendix">

## Acknowledgements
This guidelines document has been jointly produced with input and feedback from all participating organisations. In addition to the editors, the following people contributed to the document at various stages of its completion:

- name name, organisation
- name name, organisation
- etc.

</section>
