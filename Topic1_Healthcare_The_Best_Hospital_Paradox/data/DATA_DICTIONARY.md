# Data dictionary: regional hospital network, inpatient stays 2023-2025

A regional health insurer covers members in four neighbouring states (Louisiana, Mississippi, Arkansas and east Texas). Its network has 240 hospitals, grouped into 12 hospital referral regions. These files hold every inpatient stay of the insurer's members at network hospitals from January 2023 to December 2025. Members are roughly one patient in six at these hospitals, so the files do not show a hospital's full volume.

The layout follows two public sources: stay-level records such as the eICU Collaborative Research Database (one row per stay, with a severity score on arrival) and the CMS hospital files (one row per hospital). The data are made up for this course. No row describes a real patient or a real hospital.

| File | Rows | One row is |
|---|---:|---|
| `hospitals.csv` | 240 | a network hospital |
| `admissions.csv.gz` | about 1.2 million | one stay of one patient at one hospital |
| `diagnoses.csv` | 84 | a principal-diagnosis code that appears in the admissions |
| `transfers.csv` | about 38,000 | a transfer arranged through the regional transfer center |
| `*_sample.csv` | up to 1,000 | the first rows of each file |

`admissions.csv.gz` is a gzip-compressed CSV file.

## How the files connect

```
hospitals.hospital_id  1 ---- many  admissions.hospital_id
diagnoses.code         1 ---- many  admissions.primary_diagnosis_code
admissions.patient_id  many - many  transfers.patient_id
hospitals.hospital_id  1 ---- many  transfers.from_hospital_id, transfers.to_hospital_id
```

A patient keeps the same `patient_id` at every network hospital, and a patient can have several stays over the three years. A newborn gets its own `patient_id` at birth.

## hospitals.csv

| Column | Type | Meaning | Values |
|---|---|---|---|
| hospital_id | text | Hospital code | H101 to H340 |
| name | text | Hospital name | |
| type | text | Kind of hospital | `research` (40: university and academic medical centers), `community` (110), `rural` (90) |
| beds | integer | Staffed inpatient beds | |
| icu_beds | integer | Staffed intensive care beds (adult, pediatric and neonatal) | 0 means no intensive care unit |
| teaching_flag | 0/1 | 1 = has residents in training | |
| critical_access_flag | 0/1 | 1 = federal Critical Access Hospital (rural, 25 beds or fewer) | |
| trauma_level | text | State trauma center level | I (highest) to IV, or `none` |
| city | text | Town where the hospital stands | |
| state | text | Two-letter state code | LA, MS, AR, TX |
| referral_region | text | Hospital referral region | LA-1 to LA-4, MS-1 to MS-3, AR-1 to AR-3, TX-1, TX-2 |
| ownership | text | Who owns the hospital | Nonprofit, Government, For-profit |

## admissions.csv.gz

| Column | Type | Meaning | Values / units |
|---|---|---|---|
| admission_id | text | Stay identifier | A followed by 8 digits |
| patient_id | text | Patient identifier, the same at every network hospital | P followed by 7 digits |
| hospital_id | text | Hospital of this stay | see hospitals.csv |
| admit_datetime | text | Date and time the stay began (local time) | as exported by the hospital's own system |
| discharge_datetime | text | Date and time the stay ended | as exported by the hospital's own system |
| admission_type | text | How the stay began | `Emergency`, `Urgent`, `Elective` (planned), `Newborn` (birth) |
| age | integer | Age at admission | whole years; whole months for children under 2 |
| age_unit | text | Unit of `age` | `years`, `months` |
| sex | text | | F, M |
| plan_type | text | The member's insurance product | Employer group, Individual, Medicaid, Medicare Advantage |
| primary_diagnosis_code | text | Principal diagnosis (ICD-10-CM) | see diagnoses.csv |
| severity_score | integer | Acuity score taken by this hospital in the first hours after the patient arrives, from vital signs and laboratory results | 0 (well) to 100 (gravely ill); blank if not scored |
| icu_flag | 0/1 | 1 = the patient spent time in an intensive care unit during this stay | |
| length_of_stay | number | discharge_datetime minus admit_datetime | days, 2 decimals |
| discharge_status | text | How the stay ended | `home`, `home_health` (home with nursing visits), `nursing_facility`, `hospice`, `left_ama` (left against medical advice), `transfer` (sent to another hospital), `died` (died during this stay) |

## diagnoses.csv

| Column | Meaning |
|---|---|
| code | ICD-10-CM code used in admissions.primary_diagnosis_code |
| description | ICD-10-CM description |
| chapter_range | ICD-10-CM chapter, as a code range (for example `I00-I99`) |
| chapter | Chapter name (for example "Diseases of the circulatory system") |

## transfers.csv

| Column | Meaning |
|---|---|
| transfer_id | Transfer identifier (T followed by 6 digits) |
| patient_id | Patient transferred |
| from_hospital_id | Sending hospital |
| to_hospital_id | Receiving hospital |
| transfer_datetime | Time the patient left the sending hospital (YYYY-MM-DD HH:MM:SS) |
| transport_mode | Ground or Air |
| reason | Reason given by the sending hospital |

The regional transfer center keeps this log. Some transfers are arranged directly between two hospitals by phone, so not every transfer appears in it. Transfers to hospitals outside the network are not in the log.

## How a transfer appears in the admissions

Each hospital opens and closes its own stay. The sending hospital's stay ends with `discharge_status = transfer`. The receiving hospital opens a new stay with a new `admission_id`, the same `patient_id`, its own `severity_score` taken on arrival, and its own `discharge_status`. A death appears on the stay at the hospital where the patient died.

## What is NOT recorded

- Deaths after discharge. `died` means died in hospital during that stay.
- Stays at hospitals outside the network, and stays of people who are not the insurer's members.
- Where a patient came from before admission (home, nursing facility, another hospital).
- Secondary diagnoses, procedures, medicines, doctors and costs.
- Any risk-adjusted or expected mortality.
- Race, ethnicity, income and home address.
