Update of the Nordic EPUB3 Validator, work items
================================================

Coordinator: Jostein  
Participants: To be determined

Open validator issues: https://github.com/nlbdev/epub3-guidelines-update/issues?q=is%3Aopen+label%3Avalidator

## Gather information

*Who*: Someone involved with ordering EPUBs from each organization

Gather information from all agencies about how they want to use the validator/migrator. For instance:

Issue: #24

- Manually and/or automatic?
- Using one of the Pipeline 2 clients (Web UI, desktop application, command line)?
- Running on a server or on personal computers?
- Use it in an XML editor like oXygen?

## Pipeline 2

*Who*: Someone who knows how to work with Pipeline 2. Maybe get help from DAISY.

Updating the Pipeline 2 script(s).

- #25: Make the nordic-epub3-dtbook-validator project build against the newest version of DP2
- #26: Update the "Nordic EPUB3 Validator" script, to allow for validation of the new guidelines. Include the new schemas separately from the old schemas, so that both the 2015-1 guidelines as well as the new guidelines can be validated with the script. For instance by having a `guidelines-version` option.

## Documentation of validation rules

*Who*: Technical personel familiar with (or able to learn) Schematron and RelaxNG

The documention of the validation rules can be included as an appendix to the guidelines. The schemas themselves can also be included, as well as instructions for how to use the Pipeline 2 script(s).

- #4: Write human-readable documentation of all current validation rules, meant for people not familiar with Schematron or RelaxNG
- #27: Make it possible to validate EPUBs outside of Pipeline 2
- #28: Write documentation for EPUB producers on how to validate the EPUBs using either Pipeline 2, or an XML editor.

## In coordination with the "Updated EPUB-guidelines" group

*Who*: The "Updated EPUB-guidelines" group

Send the human-readable documentation of the validation rules to the "Updated EPUB-guidelines" group and ask them to:

- #29: Compare with updated guidelines to make sure there's no conflicts
- #29: Make sure that everything in the new guidelines has a validation rule
- #8: Consider removing validation rules, or at least making them less strict

The "Updated Validator" group will update the validation rules and the documentation of the validation rules based on feedback from the "Updated EPUB-guidelines" group, and ask for a new review, until there's nothing more that needs to be updated
