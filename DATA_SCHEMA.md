## Dataset Schema

The Task 1 dataset is distributed as CSV. Each row represents one chest X-ray
case and contains MIMIC-CXR identifiers together with a radiologist-validated
A1–A5 structured interpretation.

The current CSV release uses `-` to represent “none” or “not applicable.”
Participants should preserve this distinction when parsing the file rather
than treating `-` as a literal clinical label.

| Column | Role | Description |
|---|---|---|
| `Dir` | Input reference | MIMIC-CXR directory containing the corresponding restricted source files |
| `DICOM_name` | Input reference | One or more DICOM filenames associated with the case |
| `A1` | Target | Initial impressions considered during the first image review |
| `A2` | Target | Anatomical location or locations of the findings |
| `A3` | Target | Thoracic level ranges associated with the observations |
| `A4` | Target | Final impressions; `-` indicates a normal final conclusion |
| `A5_1`–`A5_4` | Target | Confirmation-checklist item numbers associated with final impressions |

Some CSV fields contain quoted lists or structured values, and a case may
reference more than one DICOM image. Use a standards-compliant CSV parser.

### A1 — Initial Impressions

A1 contains all reasonable preliminary impressions considered during the
initial image review. These are candidates, not final diagnoses, and multiple
labels may be present.

The current annotation vocabulary contains 36 labels:

- Abscess
- Ascites
- Aspergillosis
- Atelectasis
- Atherosclerosis
- Bronchiectasis
- Bulla
- Cancer
- Cardiomegaly
- Chronic obstructive pulmonary disease
- Congestion
- Congestive heart failure
- Contusion
- Edema
- Effusion
- Emphysema
- Fibrosis
- Fungus
- Granuloma
- Hemothorax
- Infection
- Interstitial Lung Disease
- Lymphadenopathy
- Mass
- Metastasis
- Nodule
- Perforation
- Pleural effusion
- Pleurisy
- Pneumomediastinum
- Pneumonia
- Pneumothorax
- Pulmonary hypertension
- Thromboembolism
- Tuberculosis
- Tumor

### A2 — Anatomical Location

A2 identifies the anatomical location and, when applicable, the laterality of
each finding. A field may contain multiple locations.

The location vocabulary covers structures including:

- lung parenchyma and lobes;
- trachea and bronchi;
- pulmonary and systemic vessels;
- right and left pleura;
- mediastinum and cardiac chambers;
- diaphragm;
- ribs and other bony structures;
- subcutaneous tissue; and
- right hilum, left hilum, and subcarinal regions.

Serialized labels must be interpreted exactly as released. For example, the
current CSV may use legacy strings such as `Parenchyme`.

### A3 — Thoracic Level

A3 represents one or more thoracic-spine ranges using `begin` and `end`
values. Both values are integers from 1 to 12 and refer to T1–T12.

Example:

```text
{'begin': 6, 'end': 11}
```

This represents a range from T6 through T11.

### A4 — Final Impressions

A4 contains the final impressions after review of the available image
evidence. It uses the same 36-label vocabulary as A1.

Final normality is determined from A4:

- **Abnormal:** A4 contains one or more final impressions.
- **Normal with preliminary suspicion:** A4 is `-`, but A1–A3 contain
  preliminary observations.
- **Pure normal:** A1–A5 are all `-`.

Preliminary A1 candidates do not need to appear in A4. If A4 is `-`, all A5
fields must also be `-`.

### A5 — Confirmation Checklist

A5 records the checklist items used to confirm or support the final
impressions in A4. The CSV provides up to four fields, `A5_1` through `A5_4`.
A field may contain more than one checklist number.

The confirmation checklist contains 28 items:

| No. | Checklist item |
|---:|---|
| 1 | Trace the trachea to the carina: is there tracheal deviation? |
| 2 | Is there narrowing or widening of the tracheal diameter? |
| 3 | Is the carina widened, with an angle greater than 100 degrees? |
| 4 | Is there bronchial narrowing or cut-off? |
| 5 | Is there an inhaled foreign body? |
| 6 | Is one lung prominently larger than the other? |
| 7 | Are the apical, upper, middle, and lower lung zones symmetrical? |
| 8 | Are there areas of abnormally increased density? |
| 9 | Is the left hilum 1–2 cm higher than the right? |
| 10 | Do both hila have the expected inverted-C shape? |
| 11 | Are both hilar angles within the normal range? |
| 12 | Do the pulmonary vessels branch progressively and uniformly? |
| 13 | Are there peripheral lines that are not vessels? |
| 14 | Is there costophrenic-angle blunting? |
| 15 | Is the right hemidiaphragm higher than the left? |
| 16 | Are the left and right heart borders clearly visible? |
| 17 | Is the cardiac position approximately one-third right and two-thirds left? |
| 18 | Is the cardiothoracic ratio within the normal range? |
| 19 | Is the aortic knob diameter within the normal range? |
| 20 | Is the right pulmonary artery diameter within the normal range? |
| 21 | Is the left pulmonary artery diameter within the normal range? |
| 22 | Is there upper-mediastinal widening? |
| 23 | Are the hilar vessels clearly visible on both sides? |
| 24 | Is there a fracture or abnormal lesion of a posterior rib? |
| 25 | Is there a fracture or abnormal lesion of the clavicles, scapulae, spine, or other bony structures? |
| 26 | Is there free gas beneath the diaphragms? |
| 27 | Is there subcutaneous emphysema? |
| 28 | Is there a hiatus hernia? |

The organizers should publish the definitive mapping rule between each A4
finding and `A5_1`–`A5_4` before the formal run. In particular, the
documentation must state whether the mapping is positional or based on a
separate finding identifier.

## Normal and Abnormal Cases

A1 is deliberately broader than A4. A model must therefore predict the
structured progression from preliminary candidates to the final conclusion,
not simply copy A1 into A4.

The first 300-case release contains all three case patterns:

- abnormal cases;
- normal cases with preliminary suspicion; and
- pure normal cases.

In the first release, 77 of the 300 records use `-` in A4 and therefore have a
normal final conclusion under the current annotation rule.

## Output Semantics

Participating systems predict A1–A5 from the chest X-ray image. The output
must use the released label vocabularies and structured representations.

If evaluation is order-independent, the final evaluation specification must
also define:

- duplicate handling;
- matching between A1 findings and A2/A3 annotations;
- matching between A4 findings and A5 checklist values; and
- treatment of semantically equivalent but differently serialized values.

