# SPSM Guidelines for Production of Accessible EPUB 3

## 1 Introduction

### 1.1 Background
This guidelines document has been developed by The National Agency for Special Needs Education and Schools (SPSM), a Swedish governmental agency. One of the agency’s missions is to adapt teaching materials for children and adults with special needs.

Fundamental to the process of adapting text-based media is the role of beginning with well-structured digital content. Previously this has been achieved through XML structures as defined by the ANSI/NISO Z39.86 specification for digital talking books (DTBook). The structures specified in these guidelines, however, are based on a profile of HTML5 requiring the use of XML serialization. This ensures that content can be reliably manipulated and rendered. Moreover, the EPUB Content Documents 3.0. specification provides constructs that further ensure semantically meaningful structures.

### 1.2 Situating the Nordic Guidelines in the World of Specifications
The Nordic Guidelines work as an application of higher-level standards for the purpose of accessible EPUB 3 production at the Ordering Agencies. As such, they build on these higher-level specification documents (EPUB 3.2, EPUB Accessibility 1.0, etc.) and provide more detailed instructions for their application in the specific context of production at the Ordering Agencies.

In the other direction, the application of the Nordic Guidelines can also be further specified on lower levels, e.g. instructions on a per-Ordering Agency and/or per-title basis.

The different levels of specification can be expressed as the following hierarchy:

1. High-level specifications, e.g. EPUB 3.2, EPUB Accessibility 1.0, HTML5, etc.
2. The Nordic Guidelines
3. General Ordering Agency-specific Guidelines (usually these depart from the Nordic Guidelines, but might also contain some deviations from them)
4. Title-specific instructions, usually expressed in the form of Editing Instructions, containing further guidance on how to treat a specific title

Note that the forms of levels 3 and 4 will vary between the Ordering Agencies, and their standardisation are outside the scope of this document.

## 2 Format Requirements

### 2.1 Required EPUB Standard

Suppliers are required to refer to the specifications provided in version 3.2 of the EPUB standard. See [https://www.w3.org/publishing/epub32/epub-spec.html](https://www.w3.org/publishing/epub32/epub-spec.html).

#### 2.1.1 Accessible Publishing Knowledge Base

In addition to following the recommended version of the EPUB standard, suppliers are required to follow the recommendations in the Accessible Publishing Knowledge Base that is maintained by the Daisy Consortium: https://kb.daisy.org/publishing/.

The recommendations in the Knowledge Base may differ from the specification. As long as no conflicts occur during validation, the recommendations in the Knowledge Base are to be followed.

### 2.2 Container

The EPUB container file must be given the production number provided with the order and correspond with the identifier stored in the `dc:identifier` element located in the Package metadata.

The container archive is required to have the `.epub` extension. The file extension must be lowercase.

Note that the `mimetype` file must be archived as the first file in the Container.

#### 2.2.1 META-INF

The `container.xml` file must identify no more than one media alternative, unless indicated otherwise by SPSM.

The container.xml file shall look like this, unless indicated otherwise:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<container version="1.0"
xmlns="urn:oasis:names:tc:opendocument:xmlns:container">
	<rootfiles>
		<rootfile full-path="EPUB/package.opf" media-type="application/oebps-package+xml"/>
	</rootfiles>
</container>
```

No other files, optional or otherwise, are allowed in the META-INF directory unless specifically indicated by SPSM.

### 2.3 Publication Resources

All publication resources are required to be located in a directory called EPUB.  Publication resources other than the Package Document (`.opf`) and XHTML Content Documents are to be located in dedicated resource sub-directories.

### 2.4 Package Document

The name of the Package Document file is required to be `package.opf`. Suppliers are required to use the file extension `.opf` for the Package Document.

The package document namespace is [http://www.idpf.org/2007/opf.](http://www.idpf.org/2007/opf.) This will be also be declared in the document.

The following xml declaration must be placed at the first line of the document:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
```

Suppliers are required to apply the following attributes and values to the `<package>` element:

- `xmlns="http://www.idpf.org/2007/opf"`
- `xmlns:nordic="http://www.mtm.se/epub/"`
- `version="3.2"`
- `xml:lang="xx"` (The publication language code)
- `unique-identifier="pub-identifier"`

Required child elements to the `<package>` element are:

- `<metadata>`
- `<manifest>`
- `<spine>`

#### 2.4.1 Metadata

The following name space declaration is required to be made in the `<metadata>` element:

- `xmlns:dc="http://purl.org/dc/elements/1.1/"`

The following metadata are required to be placed in the `<metadata>`:

```xml
<dc:title id="title"> _[the title of the publication]_ </dc:title>
<dc:language> _[language code for the main language]_ </dc:language>
<dc:identifier id="pub-identifier"> _[production UID provided by the ordering agency]_ </dc:identifier>
<dc:source> _[ISBN of the source material]_ </dc:source>
<dc:creator> _[author of the source material – one element for each author]_ </dc:creator>
<dc:format>application/epub+zip</dc:format>
<dc:publisher> _[the ordering agency]_ </dc:publisher>
<dc:date> _[date of completion]_ </dc:date>
<meta property="dcterms:modified"> _[date of completion]_ </meta>
<meta property="nordic:supplier"> _[company name of supplier]_ </meta>
<meta property="nordic:guidelines">2020-1</meta>
```

Also required are schema.org accessibility metadata, [http://kb.daisy.org/publishing/docs/metadata/schema-org.html.](http://kb.daisy.org/publishing/docs/metadata/schema-org.html.) Which metadata that are relevant depend on the type of content included in the package. Use the Accessibility Checker for EPUB tool to find out which metadata are relevant, https://inclusivepublishing.org/toolbox/accessibility-checker/getting-started/. It will typically be something like this, but not necessarily exactly the same:

```xml
<meta property="schema:accessibilitySummary">Publikationen följer WCAG 2.0 nivå AA.</meta>
<meta property="schema:accessMode">textual</meta>
<meta property="schema:accessMode">visual</meta>
<meta property="schema:accessModeSufficient">textual visual</meta>
<meta property="schema:accessModeSufficient">textual</meta>
<meta property="schema:accessibilityFeature">structuralNavigation</meta>
<meta property="schema:accessibilityFeature">MathML</meta>
<meta property="schema:accessibilityFeature">alternativeText</meta>
<meta property="schema:accessibilityHazard">noFlashing</meta>
<meta property="schema:accessibilityHazard">noSound</meta>
<meta property="schema:accessibilityHazard">noMotionSimulation</meta>
```
#### 2.4.2 Manifest

In the `<manifest>` all publication resources are declared. Each resource is declared using the `<item>` element with required attributes `id`, `href` and `media-type`. Some items must also have a `properties` attribute.

The `id` is a unique identifier used to address a particular publication resource. The only formal requirement here is its uniqueness, but SPSM will require certain naming conventions to be applied.

The `href` is simply a path to the publication resource. The path will be relative to where the package document is located.

The `media-type` attribute specifies what type of resource it is and what format. Here are some of the more common examples:

- `application/xhtml+xml`
- `text/css`
- `image/jpeg`
- `application/x-dtbncx+xml`
- `application/javascript`

Refer to https://idpf.github.io/epub-cmt/v3/ for additional information about various media types.

Some resources have additional properties that need to be identified in the corresponding item in order for a reading system to be able to implement these properties. This is done by using the properties attribute. Common examples are:

- `nav` for the `xhtml` navigation document
- `cover-image` for the cover image
- `scripted` for any content document where javascripts are being used
- `mathml` for any content document containing MathML

Refer to https://idpf.github.io/epub-vocabs/package/item/ for additional information about the properties attribute.

#### 2.4.3 Spine

In the `<spine>` all content documents are listed in the correct reading order. This is done by using an `<itemref>` element for each content document and simply list them in the desired order. The `<itemref>` element is associated with the corresponding `<item>` for the content document in the `<manifest>` by using the idref attribute and setting the value to the id attribute of the `<item>` element. The idref attribute is a required attribute and the id it refers to must be unique. Here is an example:

In the `<manifest>`:

```xml
<item id="item_7" href="007-chapter.xhtml" media-type="application/xhtml+xml"/>
```

In the `<spine>`:

```xml
<itemref idref="item_7"/>
```

The `<itemref>` has an optional attribute called linear. The linear attribute can have the values yes (default) or no. If the attribute is omitted it is set to yes. The linear attribute indicates whether the referenced item contains content that contributes to the primary reading order and has to be read sequentially (yes) or auxiliary content that enhances or augments the primary content and can be accessed out of sequence (no).  Examples of auxiliary content include: notes, descriptions and answer keys.

If a fall-back ncx navigation document is included, this is required to be referenced by adding the toc attribute to the `<spine>` element and assign as value the id attribute of the `<item>` referring to the ncx file. For example:

```xml
<spine toc="ncx">
```
### 2.5 Content Documents

#### 2.5.1 XHTML

The XHTML content files specified by the EPUB 3.2 specification are based on HTML5. Suppliers are required however to use the extension `.xhtml`.


**2.5.1.1 XML Declaration and Encoding**

The following xml declaration must be used:

```xml
<?xml version="1.0" encoding="utf-8"?>
```

**2.5.1.2 Document Type Declaration**
The following document type declaration must be included:

```html
<!DOCTYPE html>
```
**2.5.1.3 HTML Root Attributes**

Suppliers are required to include the following attributes on the html root element:

- `xmlns` – XML namespace
- `xmlns:epub` – EPUB namespace
- `xml:lang` – XML Language definition
- `lang` – HTML Language definition

**2.5.1.4 Namespaces**

The following namespace values are required to be applied to the namespace attributes:

- `xmlns="http://www.w3.org/1999/xhtml"`
- `xmlns:epub="http://www.idpf.org/2007/ops"`

**2.5.1.5 Language Definition**

Suppliers are required to identify primary languages for each content file instance using the `xml:lang` and `lang` attributes. The values for `xml:lang` and `lang` must be the same.

If not requested otherwise by the Ordering Agency, language should be coded using values from the [https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry](IANA registry of valid language codes).

Please note that the most granular language tag should be used, e.g. the macrolanguage value "no" for Norwegian could be used, but only in instances where the sublanguages Norwegian Nynorsk ("nn") or Norwegian Bokmål ("nb") are not known.

#### 2.5.2 Title

The `<title>` element of every `xhtml` content document must match the `dc:title` metadata of the package file.

#### 2.5.3 Metadata

The content documents are required to contain two `<meta>` elements.

```html
<meta name="dc:identifier" content=" _[production UID]_ "/>
<meta name="viewport" content="width=device-width"/>
```

The production UID must match the `dc:identifier` in the package.


### 2.6 Navigation Documents

#### 2.6.1 EPUB 3.2 Navigation Document

The principle navigation document of the EPUB package is the `xhtml` file with the `properties` attribute set to nav in the `<manifest>` section of the package document.  For matters of convenience mostly, this file is required to be named `nav.xhtml`.

##### 2.6.1.1 The Table Of Contents

The first required `<nav>` element in the file is:

```html
<nav role="doc-toc" aria-label="Innehållsförteckning" epub:type="toc" id="toc">
```

All headings in the main body of text must be included in the `<nav role="doc-toc"...>` element and the heading levels must be implied through nesting. Headings of sidebars, text boxes or other secondary content should not be included, unless specific instructions are given by the Ordering Agency.

The links must always reference the corresponding section element for the heading in the content file, not the h[x] element directly. Thus, in the example below, the link should point to the id "level3_2" in that content file:

```html
<section aria-labelledby="h3_2" id="level3_2">
	<h3 id="h3_2">DET MYTISKA NORDEN</h3>
```

A section without heading should be referenced using its `aria-label` value.

Note that this is not a representation of the table of contents in the source material. Only headings, or references using the `aria-label` value of the corresponding `<section>` element, should be included.

See the following link for further information (but disregard examples of unlinked headings or hidden branches):

[http://kb.daisy.org/publishing/docs/navigation/toc.html](http://kb.daisy.org/publishing/docs/navigation/toc.html)

##### 2.6.1.2 Page List

If the source material contains pagination, the next required `<nav>` element is: 

```html
<nav role="doc-pagelist" aria-label="Svartskriftssidor" epub:type="page-list">
```

See the following link for further information:

[http://kb.daisy.org/publishing/docs/navigation/pagelist.html](http://kb.daisy.org/publishing/docs/navigation/pagelist.html)

If no pagination occurs in the source material this may be omitted.

##### 2.6.1.3 Landmarks

The final required `<nav>` element is:

```html
<nav role="navigation" aria-label="Landmarks" epub:type="landmarks">
```

The Landmarks section should contain references to any main sections of the front- and rearmatter parts of the source material, as well as a reference to the start of the main content of the book, which will be the start of the first content file with the word "bodymatter" in its `epub:type` attribute of the top-level `<section>` element. The list of references may include the following, when applicable:

- Table of Contents (the toc of the source material)
- List of Images
- List of Tables
- Acknowledgments
- Preface
- Start of Content
- Answers
- Glossary
- Bibliography
- Index

Note that the links, the `<a>` elements, in the Landmarks section must have appropriate `epub:type` values. Examples can be found here:

[http://kb.daisy.org/publishing/docs/navigation/landmarks.html](http://kb.daisy.org/publishing/docs/navigation/landmarks.html)  

#### 2.6.2 NCX Navigation Document

The EPUB package may include an `ncx` navigation document as fall-back for older reading systems that have not implemented functionality for the EPUB 3 navigation document. It is not required by the EPUB specification, but it may be requested by the Ordering Agency. No `ncx` file should be included unless specifically requested. If requested, the file is required to be named `nav.ncx`.

Refer to [http://www.idpf.org/epub/20/spec/OPF_2.0.1_draft.htm#Section2.4.](http://www.idpf.org/epub/20/spec/OPF_2.0.1_draft.htm#Section2.4.) for information about how the `ncx` file should be formed. Required elements are:

- `<head>`
- `<docTitle>`
- `<navMap>`
- `<pageList>`

The `<head>` element must contain the following `<meta>` element:

```html
<meta name="dtb:uid" content=" _[production UID provided by the ordering agency]_ "/>
```

All headings must be included in the `<navMap>` and the heading levels must be implied
through nesting.

### 2.7 Images

Image content is required to be captured in the `jpeg` or `png` formats. The format extension is required to be `.jpg` or `.png`. Photos, drawings and illustrations must be captured in the `jpeg` format. For line graphics, charts and diagrams the preferred format is `png`.

Suppliers are required to maintain the highest possible image quality. The quality
requirements are the following:

- The aspect ratio of the original image must always be maintained.
- Colours must be reproduced and rendered accurately.
- The image must be free of any compression artefacts or any distortion effects arising from scanning.
- Text in images, line graphics and small detail must be crisp and fully legible, or with no degradation in legibility in comparison with the original image.
- If the `jpeg` format is used, the quality setting is required to be set at around 90% (or a corresponding setting, if a different scale of measurement is used). A setting of 100% is usually unnecessary as it yields substantially larger files with little or no visible improvement of quality. If, however, there would be visible difference in quality between 90% and a higher setting, then the higher setting should be used.

Image files are required to be stored in a folder named _images_ placed at the same level relative to the package document. The images folder must not contain subfolders.

#### 2.7.1 Resizing of Images

The maximum image size is set to 800 pixels on the longest side of the image, unless an increase in the size of an image is required to achieve the legibility of text rich images, see point 4 above, or other small detail.

In those circumstances where this requirement conflicts with requirements for legibility, the Supplier is required to contact SPSM.

#### 2.7.2 Image editing

Images are required to be captured exactly as they are in the scans or publisher files provided by SPSM. There is generally no need for editing images to enhance them above the quality of the originals (e.g. cloning to smooth edge of images spanning over spread, enhancing bad scans or old photos with bad quality already in print, etc.).


When whole sections or parts of text is placed on top of a background image, the text may need to be edited out of the background, if possible. However, no advanced restoration of lost image details are required. Note that if a publisher file is provided, images and text may have been placed in different layers and the image can be extracted without any text.

### 2.8 CSS

Suppliers are required to include the standard `css` file issued by SPSM. The `css` file is required to be stored in a folder named _css_ and placed at the same level relative to the package document.

The `<link>` element is required to be applied to the relevant content documents.

### 2.9 Fonts

Fonts present in PDF source material must not be included in the EPUB, unless specifically indicated by SPSM.

Suppliers must not include fonts in the EPUB unless requested otherwise by SPSM.

Fonts, when present, must be stored in a folder named _fonts_ and placed at the same level relative to the package document.

### 2.10 Javascript

Javascript files requested by SPSM are required to be stored in a folder named _javascript_ and placed on the same level relative to the package document.

The `<script>` element is required to be applied to the relevant content documents.

Suppliers must not include javascript files in the EPUB unless specifically instructed to do so by SPSM.

### 2.11 Validation

Validation of EPUB files should be done using EPUBCheck for structural validity and conformance to the EPUB standard and the Ace tool for accessibility evaluation. The latest distributions of these tools should be used. The tools can be found here:

- https://github.com/w3c/epubcheck/releases
- https://inclusivepublishing.org/toolbox/accessibility-checker/getting-started/

## 3 General Requirements for Content Documents

The EPUB Content File structure specified in these guidelines is generally made up of a multi-page HTML file set. Major divisions of the publication are to be captured in individual XHTML content files. The individual content files will typically correspond to _Part_ or _Chapter_ divisions of the book. Other major book components such as colophon, index or appendix, which can be found in front-matter and back-matter, will normally be stored in separate files as well.

The structural divisions of the publication are required to be semantically inflected by wrapping every structural part of the main text body in `<section>` elements, with proper nesting of subsections. Furthermore, an appropriate value of the ARIA `role` attribute, as well as the appropriate `epub:type` attribute, must be applied to the `<section>` element for the top level(s) of each content file. Usually, a subsection ends whenever a new heading of the same level or higher occurs in the source material. However, there may be situations where a `<section>` element needs to be closed sooner. If so, this should be specified in the Editing Instructions given by the Ordering Agency. If the structure of the source material is unclear and the Editing Instructions do not provide answers, please contact the Ordering Agency. 

The `role` attribute is required for the top level `<section>` element of each content file and must have a relevant value out of the following list:

https://www.w3.org/TR/dpub-aria-1.0/#roles

Whenever a `role` attribute is set to a `<section>` element, the `<section>` element is also required to have a label. When the `<section>` has a header, usually `<h1>` or `<h2>`, the header serves as a label but must be associated with the `<section>` element.  This is done by using the `aria-labelledby` attribute and setting the `id` of the associated header as value:

```html
<section role="doc-chapter" aria-labelledby="hd01" epub:type="bodymatter chapter">
	<h1 id="hd01">Chapter 1</h1>
		...
</section>
```

If the section if untitled, use the `aria-label` attribute instead and assign to it an appropriate label. This will usually be addressed in the Editing Instructions.

See [http://kb.daisy.org/publishing/docs/html/sections.html](http://kb.daisy.org/publishing/docs/html/sections.html) for more information.

The `epub:type` attribute is also required for the top level `<section>` element of each content file and must first contain one of the following partition values:

- `cover`
- `frontmatter`
- `bodymatter`
- `backmatter`

After the partition value the `epub:type` value must contain one division or sectioning value found here:

https://idpf.github.io/epub-vocabs/structure/

The exception to this is the content file containing the cover image of the printed book, which is required to have simply `epub:type="cover"`.

If an EPUB file contains both parts and chapters and each part and all its chapters is contained in a single content file, the above also is required for the second level `<section>` elements, which will contain the chapters.
	
### 3.1 File Naming Convention

The basic scheme for naming individual files is:

```
[UID]-[XXX]-[role].xhtml
```

The _UID_ must be identical to the value of `dc:identifier` metadata element. _XXX_ is a three-digit numeric string corresponding to the order in the `<spine>`, with leading zeros as needed. _Role_ corresponds to the ARIA `role` of the main section element in the content file, minus the "`doc-`" part.

Example:

```
X41001A-010- chapter.xhtml
```

### 3.2 Special Content Requirements

Some content files have certain contents that are required to be included and marked up correctly.

#### 3.2.1 Cover

The following class attributes must applied to the child `<section>` elements where appropriate:

- `<section class="frontcover">`
- `<section class="rearcover">`
- `<section class="leftflap">`
- `<section class="rightflap">`

The front cover, when available, must be captured as a `.jpg` or `.png` image file and given the name `cover.jpg/png`. The `properties="cover-image"` attribute must be applied to the manifest item element corresponding to this image. The `<img>` element referencing the cover image must have `aria-role` set to `"doc-cover"`.

Note that the `linear="no"` attribute must be applied to the `itemref` element in the package spine corresponding to this content file.

#### 3.2.2 Title page

Content corresponding to the publication’s full title must be included in an `<h1>` element.

```html
<h1 epub:type="fulltitle" class="title" id=" booktitle">
```

Use of the `<span>` element is only required when a subtitle is present. The following attribute usage must be applied to the appropriate `<span>` element:

- `<span epub:type="title">`
- `<span epub:type="subtitle">`

#### 3.2.3 Parts

Some publications are divided into parts, each containing a number of chapters. If possible, each part and its chapters should be contained in a single content file. The part heading will be the `<h1>` and the chapter headings `<h2>`. Semantic attributes, `role` and `epub:type`, must be given to the `<section>` elements of both part and chapters.

In the case of very large publications, where the size of the content files containing the parts and their chapters start to inhibit the performance of reading systems, chapters must be given their own content files. The part heading and any associated contents must then be placed in a separate content file and the `<section>` element associated with the part heading must be closed at the end of that file. Each chapter of the part must then have its own content file. Note that chapter headings must still be `<h2>`, even though they are the first heading of the content file. Semantic attributes are given to both parts and chapters as usual.

Note that this alternative option is only to be used if cleared by SPSM.

### 3.3 Mark-up Requirements

These guidelines will not give highly detailed descriptions of how to correctly handle general content. Common recommendations for making valid and accessible HTML content will apply. As a general rule, the simple solution is almost always the best solution. Using common elements like `<p>`, `<blockquote>`, `<aside>`, `<ul>`, `<ol>`, `<dl>`, `<table>`, `<figure>` etc. and the common structural elements like `<section>` and headings will almost always be sufficient. More specialised elements, or special attributes, may be needed occasionally, though. Some of the more common cases will be described below, and more obscure ones will be covered specifically in Editing Instructions.

For further information about the common HTML elements and their attributes, please refer to any online resource on HTML 5, for instance:

https://www.w3schools.com/

#### 3.3.1 Pagination

All page breaks occurring in the source copy are required to be indicated with one of the following elements unless stated otherwise by SPSM:

- Inline: `<span epub:type="pagebreak" role="doc-pagebreak">`
- Other: `<div epub:type="pagebreak" role="doc-pagebreak">`

Additionally required attributes:

- `title=""`
- `class=""`
- `id=""`

The value for the `title` attribute must be identical to the page number in the source copy. For empty pages occurring for example between chapters, the title attribute must have a value corresponding to the number implicit for that page.

In those cases where pagination of a text cannot be effectively represented using the following rules, the Supplier is required to contact SPSM.

The class attribute must have either of the following values:

- `page-front`
- `page-normal`
- `page-special`

The attribute value `page-normal` is required for the pagination of the main contents of the book, basically the bodymatter, but can also be used for the frontmatter and backmatter if the pagination is continuous.

The attribute value `page-front` is required when the page numbering series of the frontmatter is not logically continued in the publication bodymatter, i.e. roman numerals.

The attribute value `page-special` is required for any parts of the book not numbered in a standard manner, for example appendices, suites of photography etc.

The attribute `id` is simply a unique identifier.

By default, the pagebreak elements are required to be empty:

- Inline: `<span epub:type="pagebreak" role="doc-pagebreak"></span>`
- Other: `<div epub:type="pagebreak" role="doc-pagebreak"></div>`

However, there may be agency specific instructions to place the page number, as it is displayed in the source material, as content in the pagebreak elements. For example:

```html
<div class="page-normal" role="doc-pagebreak" id="page-38"
title="38">38</div>
```

This is only to be done if specific instructions are given by the Ordering Agency.

#### 3.3.2 Figures

All proper figures, illustrations, photographs, icons and other symbols must be captured as images and stored in the EPUB file, unless other instructions are given (see section 2.7). Purely decorative graphics that have no other purpose than layout can be ignored. If there are any doubts about whether to include certain graphics or not, the Supplier is required to contact SPSM.

Unless the image occurs inline in the source material, any image is required to be placed inside a `<figure>` element. If the image has a caption, the caption is required to be marked up with the `<figcaption>` element and placed directly above the `<img>` element, thus being the first child of the `<figure>` element. If no caption is present, the `<img>` element will be the first child.

Inline images, typically small symbols or other non-typographical content that might occur inline, are required to be represented with `<img>` elements alone, without the `<figure>` container. There may be cases where it is unclear whether to regard symbols or icons as inline or not, for instance small icons in connection with exercises. Specific instructions how to handle these may be given in Editing Instructions. If doubt remains, the Supplier is required to handle the image as a block element and use `<figure>` or, alternatively, contact SPSM.

Images in tables or lists may be handled as inline images if there are no captions or similar.

##### 3.3.2.1 Text Extraction from Images

When images contain text that is integral to the image itself, i.e. not a caption or similar, this text is required to be extracted as accessible text. This text must be placed in a placeholder within the `<figure>` element of the image. The placeholder should be placed after the `<img>` element, before the closing `</figure>` tag. Suppliers are required to use the `<aside>` element for this placeholder with the following attributes:

- `class="fig-desc"`
- `id=""`

where `id` is a unique identifier. Furthermore, the corresponding `<img>` element must then be given the following attribute:

- `aria-describedby=""`

with the same value as the id of the `<aside>` placeholder.

The extracted text is then placed inside the placeholder, marked up correctly and placed in a logical reading order, if there is any. Here is an example of how a figure with extracted text should be handled:

```html
<figure>
	<figcaption>...</figcaption>
	<img src="images/img0012.jpg" alt="figur" aria-describedby="desc0012" />
	<aside class="fig- desc" id="desc0012">
		<p>...</p>

	</aside>
</figure>
```

Note that this is the construction that is used by several Ordering Agencies for adding image descriptions in post-production.

##### 3.3.2.2 Alt-texts

Accessibility guidelines require all images to be supplied with a short, descriptive text as value of the `alt` attribute of the `<img>` elements. Suppliers are not required to provide these descriptive texts, but should instead use one of the following generic values:

- `foto` – for photographs
- `illustration` – for illustrations
- `figur` – for schematic images, graphs, diagrams, scientific models etc.
- `symbol` – for icons, signs, inline images etc.
- `karta` – for maps

If there are any doubts about which value to assign the `alt` attribute, Suppliers are required to use `figur`.

#### 3.3.3 Lists

Lists are a number of connected items (single words, sentences or whole paragraphs) written consecutively. They can be numbered or unnumbered. Any such content is required to be marked up with either `<ol>` (ordered list) or `<ul>` (unordered list). Each item of any list must be marked up with `<li>`.

A list item may either contain inline content or block elements, but not a mixture of both. As a rule of thumb, if all items in the list consist of single words or short phrases no further block elements are needed. If one or more of the list items consist of sentences or paragraphs, use one or more `<p>` elements inside every list item of the list.

##### 3.3.3.1 Numbered Lists

The numbering of an ordered list must not be included as content in the `<li>` elements of the list. The numbering will be rendered by the reading system. The default type for the numbering is numeric. This can be changed by using the `type` attribute. The default starting point is `1` (regardless of which type of numbering the ordered list uses), but can be changed using the `start` attribute.

##### 3.3.3.2 Unnumbered Lists

Unnumbered lists often have some sort of bullet markers for each list item. The default is a solid black circle. The type attribute has been used before, to change the type of bullet symbol, but this is not supported in HTML 5. Using CSS is the proposed method of controlling this. Suppliers are not required to modify the CSS file to match the bullet markers of the source material unless specifically instructed to do so.

Lists without any bullet markers are required to have the attribute:

- `class="plain"`

By default, `<ul>` should be used here, but Ordering Agencies may give specific instructions to use `<ol>`.

#### 3.3.4 Tables

All tables or table-like structures are required to be marked up as `<table>`. If the table has a caption it is required to be marked up with `<caption>` and placed just after the starting tag of the `<table>` element.

The `<tbody>` element is required to be used for containing the main body of table data. It is recommended, although not formally required, to use `<thead>` for any column headers at the top of the table. For the sake of consistency, it is required to use `<thead>` if there are at least one row of column headers at the top of the table, unless specific instructions are given to omit it. The element `<tfoot>` can be used if there are, for instance, a row at the bottom of the table where columns are summed up. This is not required, but can be included in Editing Instructions.

Tables are required to have a consistent number of table cells per row. If `colspan` or `rowspan` are used it is important to take note of that, so that the total number of cells is correct. 

Never use tables solely for the purpose of mimicking the layout of the source material.  The `colspan` and `rowspan` attributes may be used with `<td>` or `<th>` elements, if necessary, but if the purpose of the layout in the source material is unclear and no instructions are given, Suppliers are required to contact the Ordering Agency for clarification.

#### 3.3.5 Definition Lists

All paired lists of words, phrases, expressions etc. and corresponding definitions, translations etc. are required to be marked up as `<dl>`. Note that language attributes may be required, for example with glossaries.

#### 3.3.6 Notes and Note References

Footnotes and endnotes are required to be handled following the recommendations in the Accessible Publishing Knowledge Base:

[http://kb.daisy.org/publishing/docs/html/notes.html](http://kb.daisy.org/publishing/docs/html/notes.html)

Footnotes are to be placed in an `<aside>` at the end of the current `<section>`. If there are more than one note reference in the section notes must be grouped together in a single `<aside>`.

Backlinks are required for each note, done according to the examples in the Knowledge Base. Unless other instructions are given, the text of the backlink in endnotes should be in Swedish:

```html
<p><a href="#ref" role="doc-backlink">Gå till notreferensen.</a></p>
```

Note references are not to be marked up with `<sup>`. This is handled by the default CSS.

#### 3.3.7 Sidebars, text boxes etc.

The `<aside>` element is required to be used for any material that is placed in the margin, breaks the flow of the main text or is in some other way to be considered optional or non-essential.

Any material that is in some way contained or has a clearly visible beginning and end, but is still an integral part of the main text or content is required to be marked up with `<div>`. This could be, for instance, examples, definitions, tips, boxes containing words or phrases in exercises etc.

In both cases the elements are required to have a `class` attribute. The default value of the class attribute is text-box, but other values can be given in Editing Instructions.

For certain publications there may be instructions given to add `aria-label` attributes to either of the elements, but unless such instructions are given no `aria-label` attributes are to be included.

If there are any doubts which element to use and there are no further instructions available in the Editing Instructions, Suppliers are required to contact SPSM.

Headings in text boxes are required to be marked up with a `<h[x]>` element. Unless specific instructions are given, these headings should not be included in the navigation document.

#### 3.3.8 Computer Code

Suppliers are required to mark up code content with the `<code>` element. For block instances containing several lines of code, the `<code>` element must be contained in a `<pre>` element.

In blocks of computer code, spaces and empty lines must be preserved.

#### 3.3.9 Bolding and Italics

The only elements to be used are `<strong>` for bold and `<em>` for italics. Other type of formatting may be required in certain publications and, if such is the case, specific instructions will be given in the Editing Instructions.

Principles for how `<strong>` and `<em>` are used can be found here:

[http://kb.daisy.org/publishing/docs/html/emphasis.html](http://kb.daisy.org/publishing/docs/html/emphasis.html)

Please take care that text marked up with em or strong is identical with the original book. Do not include space or punctuation which is not emphasised in the original. Words in em or strong at the end of sentences should not include end-of-sentence punctuation, like full stops, unless the whole sentence is emphasised. If a sentence ends with an emphasised word, and the next sentence begins with a new emphasised word, make sure the words are marked up using separate instances of em or strong, and that the end-of-sentence punctuation is not included.

#### 3.3.10 Poetry and Verse

Poetry, song lyrics or any content written in verse, where lines of text must be preserved just as they are in the source material, is required to be marked up with `<div class="verse">`.

Each group of lines of text must be contained in a separate `<p class="linegroup">` element and each line of text must be marked up with `<span class="line">`. Line breaks must be added between consecutive lines within a line group.

Line numbers must only be included if specific instructions are given about it, even if they are present in the source material. If line numbers are to be included, they must be marked up with `<span class="linenum">`.

## 4 Specific Requirements for Advanced Content

### 4.1 Structural Semantics for Educational Content

The EPUB for Education Structural Semantics documentation (see [http://www.idpf.org/epub/profiles/edu/structure/)](http://www.idpf.org/epub/profiles/edu/structure/)) contains a quite elaborate structural semantic vocabulary for educational material.

### 4.2 Mathematical Content

All mathematical or scientific content, content that contains mathematical operators, special characters, subscripts, superscripts etc., must be marked up using MathML.  These guidelines will not go into much detail about the MathML language. Please refer to the specification:

https://www.w3.org/TR/MathML3/

For more detail about the MathML structure, please refer to each individual agency's requirements.

### 4.3 Special Characters, Unicode and Phonetics
If not requested otherwise by the Ordering Agency, Unicode characters should be represented by the correct Unicode character rather than an entity reference using e.g. decimal or hexadecimal notation. An inverted question mark, used in for example Spanish, should be represented as "¿" rather than "&#191;", "&#xbf;", or similar.

Note that entity reference coding of special characters may be requested on a per-production or per-Ordering Agency basis.

Unicode character accuracy for special characters, e.g. phonetic characters, is very important. A visual likeness to the characters used in the printed source material is not enough. In order for assistive technology to present correct information to the user, the correct Unicode characters must be used. Also in various conversions to other types of end-user formats, it is vital to have the correct Unicode characters in the input material.

Here are some resources for commonly used special characters:

- Phonetics: https://www.phon.ucl.ac.uk/home/wells/ipa-unicode.htm
- The Greek alphabet: https://www.fileformat.info/info/unicode/block/greek_and_coptic/list.htm



### 4.4 Placeholders for user input areas

In educational material, especially for younger children, it is common that the user is supposed to answer questions or solve problems by writing directly in the printed book.  Usually, this is indicated by printed lines where the user can write text or boxes that can be ticked etc.
