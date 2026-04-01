# CRAB (CRAB rapidly annotates in browsers) (plugin-crab)

A serverless, browser-based tool for collaborative linguistic annotation.

Ideally hosted on GitHub Pages, with 1 repository in which both your instance of CRAB and your annotations are saved (any other hosting service or even using it as local files also works).

## Setup

1. use the [template](https://github.com/git-smh/CRAB-template) - if hosting on GitHub Pages, put it in a repository and call it `<yourUsername>.github.io`.

    structure of template:

   `.github/workflows/` (folders, contain GitHub Actions workflow file)

    `jspsych/` (folder, contains CRAB and jsPsych code and css)

    `index.html` (contains your CRAB instantiation)

2. complete `index.html` - add your dataset etc. Refer to the section on parameters below.
3. create repository-scoped access token with these permissions: read access to repository metadata, and read and write access to GitHub Actions. Share this token with anyone who is to save annotations.

## Features

The toolbar at the top contains (left to right):
+ dataset overview panel: lists all documents
+ annotation guidelines
+ keyboard shortcuts
+ progress bar: shows how many documents are labelled
+ rapid mode: when labelling via shortcut, automatically advances to the next document; only works in single-label mode (each document can only have 1 label)
+ navigation: previous & next document
+ save

Below the toolbar:
+ label selectors

Centre:
+ current document
+ its metadata: its position in the list of all documents, its id, its label (labels are internally represented as integers starting from 0 in the order in which they are provided at setup)

The following are saved using browsers' `localStorage` and persist across sessions. Clear cookies and site data to reset.
+ annotations
+ current document
+ keyboard shortcuts
+ annotator name
+ access token

Saving to GitHub:
+ each annotator gets a branch named after them
+ annotations are saved in the folder `annotations` as a JSON file called `YYYY-MM-DD_HH-MM-SS_annotatorName.json` (e.g. `2026-03-23_16-08-50_jane-smith.json`)
+ a pull request to merge to main branch is created automatically

## Parameters / what to provide at setup

In addition to the [parameters available in all plugins](https://www.jspsych.org/latest/overview/plugins#parameters-available-in-all-plugins), this plugin accepts the following parameters. Parameters with a default value of undefined must be specified. Other parameters can be left unspecified if the default value is acceptable.

| Parameter          | Type                  | Default Value        | Description                                                                                                                                                                                                                                                       |
|--------------------|-----------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| stylesheet         | string                | "jspsych/crab.css"   | stylesheet for styling the GUI. Use default stylesheet, modify it, or provide your own stylesheet.                                                                                                                                                                |
| dataset            | array of JSON objects | undefined            | the dataset to annotate, which can already have labels, e.g. `[{"id": 0, "text": "text 0", "label": 0}]`. Minimally expected to have `id` and `text`, but can have additional fields. If multi-labels are allowed, already provided labels should be arrays `[]`. |
| labels             | array of strings      | undefined            | The labels to label the dataset with, e.g. `["label0", "label1"]`.                                                                                                                                                                                                |
| multi_labels       | boolean               | false                | Whether data can be annotated with multiple labels. If enabled, labels are saved as arrays `[]` instead of integers.                                                                                                                                              |
| guidelines         | HTML string           | "No guidelines."     | Annotation guidelines for annotators.                                                                                                                                                                                                                             |
| keyboard_shortcuts | JSON object           | see below            | Keyboard shortcuts.                                                                                                                                                                                                                                               |
| owner              | string                | undefined            | Username of the GitHub account which owns the repository.                                                                                                                                                                                                         |
| repo               | string                | undefined            | Name of the GitHub repository to save annotations to.                                                                                                                                                                                                             |
| workflow           | string                | save-annotations.yml | Name of the GitHub Actions file, which does the saving to GitHub. Use default file, modify it, or provide your own file.                                                                                                                                          |

default keyboard shortcuts:
```
{
all_items: "a",
guidelines: "g",
keyboard_shortcuts: "h",
rapid_mode: "r",
prev: "j",
next: "k",
save: "s",
labels: ["1", "2", "3", "4", "5", "6", "7", "8", "9"],
},
```

## Data Generated

In addition to the [default data collected by all plugins](https://www.jspsych.org/latest/overview/plugins#data-collected-by-all-plugins), this plugin collects the following data for each trial.

Note that CRAB does not use jsPsych's saving mechanism but browsers' `localStorage`, which persists across sessions, and saves annotations to GitHub.

| Name              | Type        | Value                       |
|-------------------|-------------|-----------------------------|
| annotator         | string      | The entered annotator name. |
| annotated_dataset | JSON object | The annotated dataset.      |

## Example

```javascript
const trial = {
  type: jsPsychCrab,
  stylesheet: "../src/crab.css",
  dataset: [
    {"id": 0, "text": "This is a demo of CRAB.\n\nIMPORTANT: saving to GitHub will not work as no repository has been specified (and you do not have a corresponding access token)."},
    {"id": 1, "text": "This demo of CRAB is in single-label mode. Rapid mode is available. It is not available in multi-label mode."},
    {"id": 2, "text": "This item already came with a label assigned.", "label": 0},
    {"id": 3, "text": "Lorem ipsum dolor sit amet, consectetuer adipiscing elit."},
    {"id": 4, "text": "Aenean commodo ligula eget dolor. Aenean massa."},
    {"id": 5, "text": "Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus."},
    {"id": 6, "text": "Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim."},
    {"id": 7, "text": "Donec pede justo, fringilla vel, aliquet nec, vulputate eget, arcu. In enim justo, rhoncus ut, imperdiet a, venenatis vitae, justo."},
    {"id": 8, "text": "Nullam dictum felis eu pede mollis pretium."},
    {"id": 9, "text": "Integer tincidunt."},
    {"id": 10, "text": "Cras dapibus."},
    {"id": 10, "text": "Vivamus elementum semper nisi."},
  ],
  labels: ["not ironic", "ironic"],
  guidelines: "Please read and adhere to these guidelines:<ol><li>They can be styled with HTML.</li><li>Aenean vulputate eleifend tellus.</li><li>Aenean leo ligula, porttitor eu, consequat vitae, eleifend ac, enim.</li></ol>",
  owner: "theOwner",
  repo: "theRepo",
};
```
