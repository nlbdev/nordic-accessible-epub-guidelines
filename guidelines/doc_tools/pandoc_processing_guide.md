# Pandoc processing guide for the Nordic Guidelines
Below are notes for how the final guidelines documents are generated from the master ~guidelines.md~ file, with help from the super awesome document Swiss army knife [Pandoc](https://pandoc.org/).

With a little help from Pandoc, the master document can be processed with nice HTML and PDF results, to be published at https://format.mtm.se/nordic_epub.

The instructions assume Pandoc is called from the working directory `[repository root]/guidelines`.

## Resources
- guidelines.md: The master file with the guidelines content
- doc_tools/metadata.yaml: Metadata in a separate file to be passed to Pandoc
- doc_tools/template-html.html: HTML template, slightly modified from the Pandoc default, including both structure and some embedded CSS rules

## Generating the HTML output
(Pandoc is currently issued in a Windows environment)

```
pandoc -f markdown -t html .\guidelines.md .\doc_tools\metadata.yaml -o .\generated_docs\2020-1\nordic_guidelines_epub3-2020-1.html --toc --toc-depth=4 -V lang=en --template .\doc_tools\template-html.html --shift-heading-level-by=-1 -V current_date="$(date +%Y-%m-%d%n)" --number-sections
```

## Generating the PDF output
