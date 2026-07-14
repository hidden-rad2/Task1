# Hidden-RAD2 Task 1: Structured Radiological Reasoning Prediction

This repository provides the dataset for **Task 1 of the NTCIR-19 Hidden-RAD2 challenge**.

Hidden-RAD2 Task 1 evaluates whether an AI system can predict the
radiologist-validated structured reasoning annotations associated with
a chest X-ray image. Rather than predicting only a final diagnosis or
generating a free-text radiology report, the system must predict the
initial impressions, anatomical locations, thoracic levels or lung
zones, final impressions, and confirmation evidence represented by
A1–A5.

For the official task description, see the [Hidden-RAD2 Task 1 Definition](https://sites.google.com/view/hidden-rad2/tasks/task-1-definition).

## Dataset Release

### First release: 300 cases

The first batch of **300 annotated cases** is available in this repository:

- [`NTCIR19_Task1_Sample_1st.csv`](./NTCIR19_Task1_Sample_1st.csv)

Each case contains identifiers for the corresponding MIMIC-CXR chest X-ray image and a radiologist-validated structured interpretation consisting of A1–A5. The first release contains 300 cases. Under the current annotation rule,
77 cases have `-` in A4 and therefore represent a normal final conclusion.

Chest X-ray image files are not redistributed through this repository. Participants must obtain the corresponding images directly from MIMIC-CXR with the required PhysioNet credentials and data-use authorization.

## Task Definition

Given a chest X-ray image, a participating system must predict the
corresponding radiologist-validated A1–A5 structured interpretation.

The system must:

1. identify all reasonable initial impressions;
2. identify the anatomical locations of the findings;
3. determine the relevant thoracic levels or lung zones;
4. decide the final impressions, including a normal conclusion when appropriate; and
5. provide an ABCDE-based confirmation checklist supporting each final impression.

## Input

Each test case provides:

- a chest X-ray image from the MIMIC-CXR source dataset.

Participants may optionally use external medical knowledge resources, pretrained models, or other supporting information, subject to the challenge rules.

The corresponding medical report is used as a reference during dataset annotation, but it is **not the primary input of Hidden-RAD2 Task 1**.

## Required Output

For each chest X-ray image, participants must predict and submit the following structured output.

### A1 — Initial Impressions

List all possible impressions considered during the initial image review.

These are preliminary diagnostic candidates and do not necessarily need to appear in the final impressions.

### A2 — Anatomical Location

Identify the anatomical location of each finding, including laterality when applicable.

### A3 — Thoracic Level

Specify the relevant thoracic spine level or lung zone associated with each observation.

### A4 — Final Impressions

Provide the final diagnostic impressions supported by the image evidence.

A case may be classified as normal even when preliminary observations were considered in A1.

### A5 — Confirmation Checklist

Complete the ABCDE-based confirmation checklist for each final impression.

The checklist should identify the image findings and other supporting evidence that confirm or support the diagnostic decision.

## CSV Format

The released CSV file contains one case per row.

| Column | Description |
|---|---|
| `Dir` | MIMIC-CXR directory containing the corresponding restricted source files |
| `DICOM_name` | One or more DICOM filenames associated with the case |
| `A1` | Initial impressions |
| `A2` | Anatomical locations |
| `A3` | Thoracic-level ranges |
| `A4` | Final impressions; `-` indicates a normal final conclusion |
| `A5_1`–`A5_4` | Confirmation-checklist items associated with final impressions |

The current CSV uses `-` to represent “none” or “not applicable.”
Some fields contain quoted lists or structured values, and a case may
reference multiple DICOM images. Use a standards-compliant CSV parser.

For the complete data specification—including the 36-label vocabulary,
normal-case encoding, anatomical-location structure, thoracic-level
ranges, and the 28-item confirmation checklist—see
[DATA_SCHEMA.md](./DATA_SCHEMA.md).

## Using the Released Data

The CSV contains the structured annotations and the information needed to locate the corresponding images in MIMIC-CXR.

A typical workflow is:

1. obtain authorized access to MIMIC-CXR through PhysioNet;
2. locate each study using the `Dir` value;
3. load the DICOM image or images listed in `DICOM_name`;
4. use the image as the model input; and
5. use the corresponding A1–A5 fields as the structured target output.

The dataset should be parsed using a CSV-compatible library because some fields contain quoted lists, structured values, or multiple DICOM filenames.

## Accessing the Images

The CSV file provides MIMIC-CXR directory and DICOM identifiers but does not contain the image files themselves.

To access the corresponding chest X-ray images, participants must:

1. create a PhysioNet account;
2. complete the required human-subjects research training;
3. become a credentialed PhysioNet user; and
4. accept the MIMIC-CXR Data Use Agreement.

Please refer to:

- [MIMIC-CXR on PhysioNet](https://physionet.org/content/mimic-cxr/2.1.0/)
- [PhysioNet credentialing and training](https://physionet.org/about/citi-course/)
- [Hidden-RAD2 MIMIC-CXR Data Access](https://sites.google.com/view/hidden-rad2/tasks/mimic-cxr-data-access)

Participants are responsible for complying with all applicable PhysioNet and MIMIC-CXR data-use requirements.

## Important Notes

- This release contains **300 cases**.
- The repository distributes image identifiers and structured annotations, not MIMIC-CXR image files.
- MIMIC-CXR images and other controlled-access data must not be redistributed.
- A1 contains preliminary candidates and may differ from the final impressions in A4.
- A2 represents anatomical location, while A3 represents the thoracic level or lung zone.
- The corresponding medical report is an annotation reference and should not be treated as the required Task 1 test input.
- Additional datasets, evaluation resources, and submission instructions may be released according to the Hidden-RAD2 schedule.
- Participants should check this repository and the official challenge website regularly for updates.

## Challenge Information

- [Hidden-RAD2 website](https://sites.google.com/view/hidden-rad2/)
- [Task 1 Definition](https://sites.google.com/view/hidden-rad2/tasks/task-1-definition)
- [Dataset Information](https://sites.google.com/view/hidden-rad2/dataset)
- [Evaluation](https://sites.google.com/view/hidden-rad2/evaluation)
- [Schedule](https://sites.google.com/view/hidden-rad2/schedule)

## Citation

Citation information for Hidden-RAD2 and the NTCIR-19 task overview paper will be added when available.
