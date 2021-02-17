# Pandoc processing guide for the Nordic Guidelines
Below are notes for how the final guidelines documents are generated from the master ~guidelines.md~ file, with help from the super awesome document Swiss army knife [Pandoc](https://pandoc.org/).

With a little help from Pandoc, the master document can be processed with nice HTML and PDF results, to be published at https://format.mtm.se/nordic_epub.

The instructions assume Pandoc is called from the working directory `[repository root]/guidelines`. They also assume Pandoc is issued in a Windows environment (with MikTeX handling the LaTeX packages for PDF generation), but should be easily translated into a GNU/Linux context.

## Resources
- guidelines.md: The master file with the guidelines content
- doc_tools/metadata.yaml: Metadata in a separate file to be passed to Pandoc
- doc_tools/template-html.html: HTML template, slightly modified from the Pandoc default, including both structure and some embedded CSS rules



## Generating the HTML output
```
pandoc -f markdown -t html .\guidelines.md .\doc_tools\metadata.yaml -o .\generated_docs\2020-1\nordic_guidelines_epub3-2020-1.html --toc --toc-depth=4 --shift-heading-level-by=-1 --number-sections -V current_date="$(date +%Y-%m-%d%n)" --template .\doc_tools\template-html.html
```

## Generating the PDF output
```
pandoc -f markdown -t pdf .\guidelines.md .\doc_tools\metadata.yaml -o .\generated_docs\2020-1\nordic_guidelines_epub3-2020-1.pdf --toc --toc-depth=4 --shift-heading-level-by=-1 --number-sections -V current_date="$(date +%Y-%m-%d%n)" --template .\doc_tools\template-latex.tex --pdf-engine=xelatex 
```
