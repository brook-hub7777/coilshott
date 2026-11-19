# Repository and local workflow

[Development walkthrough](README.md)

## Read this walkthrough as a retrospective

This walkthrough documents the Nimbus Salvagers checkout as it exists today. The chapters cover repository orientation and local setup, the browser application, its interfaces and assets, validation, and deployment. The entries explain how the checked-in pieces fit together and how to work with them. Treat the sequence as a reading order. Use the original Git commits when you need evidence about when files entered the repository.

## Identify the checkout and the application

The GitHub repository is `brook-hub7777/coilshott`. Its nested tooling directory is spelled `coilshot` with one final t. Nimbus Salvagers is the title shown in the HTML and the name used by the README. In the cloud checkout, `/workspace/coilshott` is the repository root. Run `git rev-parse --show-toplevel` if you entered through a different folder, then resolve all repository-relative paths in this guide from that root.
