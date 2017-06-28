#### Heading

### Lab Orders

#### Version:

1.2.0 04-Jan-2017

Altered POST/PUT JSON to reflect updated template.

1.1.0: 04-Jan-2017

#### TemplateID:
`IDCR - Laboratory Order.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    a_a/activities[at0001]/description[at0009]/items[at0121]/value/value as name,
    a_a/activities[at0001]/description[at0009]/items[at0121]/value/defining_code/code_string as code,
    a_a/activities[at0001]/description[at0009]/items[at0121]/value/defining_code/terminology_id/value as terminology,
    b_a/description[at0001]/items[at0017]/value/value as Test_name,
    b_a/time/value as date_ordered
from EHR e[ehr_id/value='{{ehrId}}']
contains COMPOSITION a
contains (
    INSTRUCTION a_a[openEHR-EHR-INSTRUCTION.request-lab_test.v1] or
    ACTION b_a[openEHR-EHR-ACTION.laboratory_test.v1])
where a/name/value='Laboratory order'

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
  "laboratory_order/laboratory_test_request/request/service_requested|code": "444164000",
  "laboratory_order/laboratory_test_request/request/service_requested|value": "Urea, electrolytes and creatinine measurement",
  "laboratory_order/laboratory_test_request/request/service_requested|terminology": "SNOMED-CT",
  "laboratory_order/laboratory_test_request/request/timing": "R5/2015-08-12T22:00:00+02:00/P2M",
  "laboratory_order/laboratory_test_request/request/timing|formalism": "timing",
  "laboratory_order/laboratory_test_request/narrative": "Urea, electrolytes and creatinine measurement",
  "laboratory_order/laboratory_test_tracker/ism_transition/current_state|code": "526",
  "laboratory_order/laboratory_test_tracker/ism_transition/current_state|value": "planned",
  "laboratory_order/laboratory_test_tracker/ism_transition/current_state|terminology": "openehr",
  "laboratory_order/laboratory_test_tracker/ism_transition/careflow_step|code": "at0003",
  "laboratory_order/laboratory_test_tracker/ism_transition/careflow_step|value": "Test requested",
  "laboratory_order/laboratory_test_tracker/ism_transition/careflow_step|terminology": "local",
  "laboratory_order/laboratory_test_tracker/test_name|code": "444164000",
  "laboratory_order/laboratory_test_tracker/test_name|value": "Urea, electrolytes and creatinine measurement",
  "laboratory_order/laboratory_test_tracker/test_name|terminology": "SNOMED-CT"
}

```

#### Notes:

None
