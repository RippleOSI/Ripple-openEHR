#### Heading

### End of Life Care Preferences

#### Version:

1.0.0

19-Dec-2016

#### TemplateID:
`IDCR - End of Life Patient Preferences.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
  a/uid/value as uid,
  a/composer/name as author,
  a/context/start_time/value as date_created,
  b_a/data[at0001]/items[at0003]/value/value as priority_place_of_care,
  b_a/data[at0001]/items[at0015]/value/value as priority_place_of_death,
  b_a/data[at0001]/items[at0029]/value/value as priority_comment,
  b_b/data[at0001]/items[at0003]/value/value as treatment_decision,
  b_b/data[at0001]/items[at0002]/value/value as treatment_date_of_decision,
  b_b/data[at0001]/items[at0021]/value/value as treatment_comment,
  b_c/data[at0001]/items[at0003]/value/value as cpr_decision,
  b_c/data[at0001]/items[at0002]/value/value as cpr_date_of_decision,
  b_c/data[at0001]/items[at0021]/value/value as cpr_comment
 from EHR e [ehr_id/value = '{{ehrId}}']
 contains COMPOSITION a[openEHR-EHR-COMPOSITION.care_plan.v1]
 contains (EVALUATION b_a[openEHR-EHR-EVALUATION.care_preference_uk.v1]
  or EVALUATION b_b[openEHR-EHR-EVALUATION.advance_decision_refuse_treatment_uk.v1]
   or EVALUATION b_c[openEHR-EHR-EVALUATION.cpr_decision_uk.v1])
 where a/name/value='End of Life Patient Preferences'
```

#### Detail AQL /query:
To populate the detailed view / edit when a single record within the heading is selected.

```
As above
```

#### Sample Composition(FLAT JSON) for POST/PUT /composition:

To create or update a composition for a single item via the /composition Ehrscape API call.

```
{
  "ctx/composer_name": "Dr Tony Shannon",
  "ctx/health_care_facility|id": "999999-345",
  "ctx/health_care_facility|name": "Home",
  "ctx/id_namespace": "NHS-UK",
  "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
  "ctx/language": "en",
  "ctx/territory": "GB",
  "ctx/time": "2016-02-24T00:11:02.518+02:00",
  "end_of_life_patient_preferences/legal_information:0/preferred_priorities_of_care:0/preferred_place_of_care:0|code": "at0009",
  "end_of_life_patient_preferences/legal_information:0/preferred_priorities_of_care:0/preferred_place_of_death:0|code": "at0020",
  "end_of_life_patient_preferences/legal_information:0/preferred_priorities_of_care:0/comment": "Decision is settled",
  "end_of_life_patient_preferences/legal_information:0/advance_decision_to_refuse_treatment/decision_status|code": "at0028",
  "end_of_life_patient_preferences/legal_information:0/advance_decision_to_refuse_treatment/date_of_decision": "2016-12-19T17:02:31.696Z",
  "end_of_life_patient_preferences/legal_information:0/advance_decision_to_refuse_treatment/comment": "Wife aware",
  "end_of_life_patient_preferences/legal_information:0/cpr_decision/cpr_decision|code": "at0004",
  "end_of_life_patient_preferences/legal_information:0/cpr_decision/date_of_cpr_decision": "2016-12-19T17:02:31.696Z",
  "end_of_life_patient_preferences/legal_information:0/cpr_decision/informal_carer_awareness_of_decision|code": "at0026",
  "end_of_life_patient_preferences/legal_information:0/cpr_decision/comment": "Religious reasons"
}
```

#### Notes:

None
