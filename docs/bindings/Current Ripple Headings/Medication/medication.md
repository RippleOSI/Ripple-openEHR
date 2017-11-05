#### Heading

### Medication

#### Version:

2.0.0 : 26-June-2017


#### TemplateID:
`IDCR - Medication Statement List.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    b_a/activities[at0001]/description[at0002]/items[at0173, 'Dose amount description']/value/value as dose_amount,
    b_a/activities[at0001]/description[at0002]/items[at0173, 'Dose timing description']/value/value as dose_timing,
    b_a/activities[at0001]/description[at0002]/items[at0044]/value/value as dose_directions,
    b_a/activities[at0001]/description[at0002]/items[at0091]/value/value as route,
    b_a/activities[at0001]/description[at0002]/items[at0070]/value/value as medication_name,
    b_a/activities[at0001]/description[at0002]/items[at0070]/value/defining_code/code_string as medication_name_code,
    b_a/activities[at0001]/description[at0002]/items[at0113]/items[at0012]/value/value as start_date
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.medication_list.v0]
contains INSTRUCTION b_a[openEHR-EHR-INSTRUCTION.medication_order.v1]
where a/name/value='Medication statement list'
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
  "ctx/composer_name": "Dr George Shaw",
  "ctx/health_care_facility|id": "999999-345",
  "ctx/health_care_facility|name": "Home",
  "ctx/id_namespace": "NHS-UK",
  "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
  "ctx/language": "en",
  "ctx/territory": "GB",
  "ctx/time": "2016-02-24T00:11:02.518+02:00",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/medication_item|value": "Enalapril",
      "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/medication_item|code": "15222008",
        "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/medication_item|terminology": "SNOMED-CT",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/route": "oral",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/dose_amount_description": "10mg",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/dose_timing_description": "in the morning",
     "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/additional_instruction:0": "Take before food",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/order_details/order_start_date_time": "2016-02-24T10:50:42Z",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/order_details/order_summary/course_status|code": "at0021",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/order:0/timing": "R5/2017-06-26T10:00:00Z/P1M",
    "medication_statement_list/medication_and_medical_devices/medication_order:0/narrative": "Enalapril - oral - 10mg in the morning"
}
```

#### Notes:

DARFT template - will be updated to V1 when openEHR medication archetypes are published.
