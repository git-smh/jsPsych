# CRAB (CRAB rapidly annotates in browsers) (plugin-crab)

## Overview

A serverless, browser-based tool for collaborative linguistic annotation.

Ideally hosted on GitHub Pages - so your instance of CRAB and your saved annotations are in the same repository.

Features:

+ document labelling
+ keyboard shortcuts & rapid mode for efficient annotation
+ save annotations to a GitHub repository

## Demo

A [demo](https://git-smh.github.io/crab-demo.html) is available.

## Setup

1. use the [template](https://github.com/git-smh/CRAB-template) - if hosting on GitHub Pages, put it in a repository and call it `<yourUsername>.github.io`.
2. modify `index.html` - add your dataset etc. Refer to the [documentation](https://github.com/git-smh/jsPsych/blob/main/packages/plugin-crab/docs/crab-docs.md).
3. create repository-scoped access token with these permissions: read access to repository metadata, and read and write access to GitHub Actions.

## Compatibility

`plugin-crab` requires jsPsych v8.0.0 or later.

## Documentation

See [documentation](https://github.com/git-smh/jsPsych/blob/main/packages/plugin-crab/docs/crab-docs.md).

## Author / Citation

S. M. Han