#### Heading

### Medication

#### Version:

1.0.0

18-Dec-2016

#### TemplateID:
``

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    b_a/items[at0001]/value/value as name,
    b_a/items[at0001]/value/defining_code/code_string as medication_code,
    b_a/items[at0001]/value/defining_code/terminology_id/value as medication_terminology,
    b_a/items[at0002]/value/value as route,
    b_a/items[at0003]/value/value as dose_directions,
    b_a/items[at0020]/value/value as dose_amount,
    b_a/items[at0021]/value/value as dose_timing,
    b_a/items[at0046]/items[at0039]/value/value as start_date
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.care_summary.v0]
contains (
    EVALUATION a_a[openEHR-EHR-EVALUATION.medication_statement_uk.v1] and
    CLUSTER b_a[openEHR-EHR-CLUSTER.medication_item.v1])
where a/name/value='Current medication list'
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
      "ctx/health_care_facility|name": "Rippleburgh GP Practice",
      "ctx/id_namespace": "NHS-UK",
      "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
      "ctx/language": "en",
      "ctx/territory": "GB",
      "ctx/time": "2016-12-19T09:26:02Z",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/medication_name|value":"Ampicillin",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/medication_name|code": "31087008",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/medication_name|terminology": "SNOMED-CT",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/route": "Intramuscular",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/dose_amount_description": "500mg",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/dose_timing_description": "once only",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/course_details/start_datetime": "2016-12-24T08:30:00Z",
      "current_medication_list/medication_and_medical_devices:0/current_medication:0/medication_statement:0/medication_item/dose_directions_description": "give gently"
}

```

#### Notes:

DARFT template - will be updated to V1 when openEHR medication archetypes are published.
