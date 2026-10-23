# Data dictionary: district tutoring program, school year 2025-26

An export from the district's student information system, its grade book and the tutoring program's attendance log, made in October 2026. The files are as the district sent them; they have not been cleaned.

## Files at a glance

| file | rows | one row is |
|---|---|---|
| `schools.csv` | 24 | a high school |
| `school_calendar.csv` | 304 | a calendar day from 2025-08-01 to 2026-05-31 |
| `students.csv` | 40,000 | a student enrolled in grades 9-12 at the end of the 2025-26 school year |
| `courses.csv` | 32 | a course in the district catalog |
| `sections.csv` | 9,651 | a class section (one course, one teacher, one school) |
| `term_grades.csv` | 720,000 | one student, one course, one term (40,000 students x 6 courses x 3 terms) |
| `tutors.csv` | 614 | a tutor employed by the program |
| `tutoring_referrals.csv` | 14,917 | a student referred to tutoring in one subject |
| `tutoring_sessions.csv.gz` | 415,850 | one scheduled tutoring session for one student (gzip-compressed CSV) |

Files with more than 1,000 rows also come as `*_sample.csv` with their first 1,000 rows.

How the files connect:

```
schools.school_code ──< students.school_code
schools.school_code ──< sections.school_code ──< term_grades.section_id
courses.course_code ──< sections.course_code
students.student_id ──< term_grades.student_id                 (18 rows per student)
students.student_id ──< tutoring_referrals.student_id          (0 to 3 rows per student: one per subject)
tutors.tutor_id     ──< tutoring_referrals.tutor_id
tutoring_referrals (student_id, subject_area) ──< tutoring_sessions (student_id, subject_area)
school_calendar.calendar_date ── tutoring_sessions.session_date, tutoring_referrals.referral_date
```

Staff identifiers (teachers, counselors, tutors) all have the form `E` followed by six digits and come from the district's employee numbers.

## The school year

| term | first day | last day | final exams |
|---|---|---|---|
| 1 | 2025-08-06 | 2025-10-31 | 2025-10-29 to 2025-10-31 |
| 2 | 2025-11-04 | 2026-02-13 | 2026-02-11 to 2026-02-13 |
| 3 | 2026-02-18 | 2026-05-21 | 2026-05-19 to 2026-05-21 |

Holidays, breaks and teacher workdays are listed day by day in `school_calendar.csv`. There are 178 student days in the year.

Grades: the district moved to a new grade book after term 1. Term-1 grades were exported from the previous grade book as letter grades only; terms 2 and 3 are exported as percentages. The district's grading scale is A 90-100, B 80-89, C 70-79, D 60-69, F below 60. An F is a failing grade.

## `schools.csv`

| column | type | meaning | values |
|---|---|---|---|
| school_code | text | state site code: district code 417 followed by the three-digit site number | 417002 ... 417087 |
| school_name | text | | |
| locale | text | locale category of the school's address | City, Suburb, Town, Rural |
| grades_served | text | | 9-12 |
| enrollment | integer | students enrolled at the end of 2025-26 | |
| econ_disadvantaged_pct | number | percent of the school's students flagged as economically disadvantaged | 0-100 |
| title_i | text | school receives federal Title I funding | Y, N |

## `school_calendar.csv`

| column | type | meaning | values |
|---|---|---|---|
| calendar_date | date | | YYYY-MM-DD |
| day_of_week | text | | Monday ... Sunday |
| day_type | text | what kind of day it is for students | instructional, exam, holiday, break, teacher_workday, weekend, summer |
| event | text | name of the holiday, break or event; blank on ordinary days | |
| term | integer | term the day belongs to; blank on days without students | 1, 2, 3 |

## `students.csv`

| column | type | meaning | values |
|---|---|---|---|
| student_id | text | district student number | seven digits |
| school_code | text | the student's school | see schools |
| grade_level | integer | grade in 2025-26 | 9, 10, 11, 12 |
| gender | text | | F, M |
| birth_date | date | | YYYY-MM-DD |
| econ_disadvantaged | text | eligible for free or reduced-price meals | Y, N |
| english_learner | text | receives English learner services | Y, N |
| iep | text | has an individualized education program (special education) | Y, N |
| bus_rider | text | assigned to a school bus route | Y, N |
| district_entry_date | date | first day the student was enrolled in the district | YYYY-MM-DD |
| prior_year_gpa | number | unweighted grade point average for the 2024-25 school year (for grade 9, the grade-8 year) | 0.00-4.00. Blank when the previous school's transcript has not been loaded (students who entered the district in 2025) |

## `courses.csv`

| column | type | meaning | values |
|---|---|---|---|
| course_code | text | district course code | e.g. MTH110 |
| course_title | text | | e.g. Algebra I |
| subject_area | text | | Mathematics, English Language Arts, Science, Social Studies, World Languages, Health and PE, Electives |
| grade_levels | text | grade levels the course is normally taken in | e.g. 9-10 |
| credits | number | credits earned for the full year | 1.0 |
| length | text | | year (3 terms) |

Every course is year-long: a student keeps the same section and teacher in all three terms. Mathematics placement depends on the student's grade level and earlier courses.

## `sections.csv`

| column | type | meaning | values |
|---|---|---|---|
| section_id | text | school code, course code and section number | e.g. 417002-MTH110-03 |
| school_code | text | | see schools |
| course_code | text | | see courses |
| teacher_id | text | the teacher of the section | E + six digits |

## `term_grades.csv`

| column | type | meaning | values |
|---|---|---|---|
| student_id | text | | see students |
| school_code | text | | see schools |
| section_id | text | | see sections |
| course_code | text | | see courses |
| term | integer | | 1, 2, 3 |
| term_grade | text | the term grade as exported from the grade book | term 1: A, B, C, D, F; terms 2-3: whole percent 0-100 |
| absences | integer | class periods of this course the student missed during the term | 0 and up |

## `tutors.csv`

| column | type | meaning | values |
|---|---|---|---|
| tutor_id | text | | E + six digits |
| school_code | text | the school where the tutor works | see schools |
| subject_area | text | the subject the tutor teaches | Mathematics, English Language Arts, Science |
| tutor_type | text | | Certified teacher, Paraprofessional, College tutor |

## `tutoring_referrals.csv`

| column | type | meaning | values |
|---|---|---|---|
| referral_id | text | | TR25-00001 ... |
| student_id | text | | see students |
| school_code | text | | see schools |
| subject_area | text | the subject the student was referred for | Mathematics, English Language Arts, Science |
| referral_date | date | the day the referral was recorded | YYYY-MM-DD |
| referred_by | text | who asked for the student to join | Teacher, Counselor, Parent, Self |
| referring_staff_id | text | employee number of the referring teacher or counselor; blank for Parent and Self | E + six digits |
| tutor_id | text | the tutor the student was placed with | see tutors |
| session_day | text | the student's weekly tutoring day | Monday, Tuesday, Wednesday, Thursday |
| start_time | text | start time of the session | 15:45, 16:35 |

A student has at most one referral per subject. Once referred, a student stays in the program for the rest of the year; there is no leaving date.

## `tutoring_sessions.csv.gz`

| column | type | meaning | values |
|---|---|---|---|
| session_id | text | identifier of the scheduled session for this student | TS0000001 ... |
| student_id | text | | see students |
| tutor_id | text | | see tutors |
| subject_area | text | | Mathematics, English Language Arts, Science |
| session_date | date | | YYYY-MM-DD |
| start_time | text | | 15:45, 16:35 |
| minutes | integer | minutes logged by the tutor | 45 if present, 0 if absent |
| attendance | text | whether the student came | Present, Absent |

The program runs Monday to Thursday on instructional days from 2025-08-18 to the end of the year. There are no sessions on holidays, breaks, teacher workdays or final exam days. A referred student is scheduled every week on his or her session day, starting with the first session day at least three days after the referral. A tutor meets up to four students at the same time. The log is exported in the order the rows were entered, not sorted. The file has no term column; the terms are in `school_calendar.csv`.

## What is NOT recorded

- Why a student was referred. The file says who made the referral, not the reason.
- Marks inside a term (tests, homework, progress reports); only the final term grade.
- What happened in a tutoring session (topic, homework, tutor notes).
- Private tutoring, paid classes or help at home outside the district's program.
- Daily school attendance; only the absences per course per term.
- Transcripts from previous schools for students new to the district.
- Students who left the district during the year (the export lists only students enrolled at the end of the year).
