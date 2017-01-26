#### Heading

### Procedures

#### Version:

1.1.0

04-Jan-2017

#### TemplateID:
`IDCR - Procedures List.v1`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_submitted,
    b_a/description[at0001]/items[at0002]/value/value as procedure_name,
    b_a/description[at0001]/items[at0002]/value/defining_code/code_string as procedure_code,
    b_a/description[at0001]/items[at0002]/value/defining_code/terminology_id/value as procedure_terminology,
    b_a/description[at0001]/items[at0049]/value/value as procedure_notes,
    b_a/other_participations/performer/name as performer,
    b_a/time/value as procedure_datetime,
    b_a/ism_transition/current_state/value as procedure_state,
    b_a/ism_transition/current_state/defining_code/code_string as procedure_state_code,
    b_a/ism_transition/current_state/defining_code/terminology_id/value as procedure_state_terminology,
    b_a/ism_transition/careflow_step/value as procedure_step,
    b_a/ism_transition/careflow_step/defining_code/code_string as procedure_step_code,
    b_a/ism_transition/careflow_step/defining_code/terminology_id/value as procedure_step_terminology
    from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.health_summary.v1]
contains ACTION b_a[openEHR-EHR-ACTION.procedure.v1]
where a/name/value='Procedures list'
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
 "ctx/composer_name": "Dr Joyce Smith",
 "ctx/health_care_facility|id": "999999-345",
 "ctx/health_care_facility|name": "Northumbria Community NHS",
 "ctx/id_namespace": "NHS-UK",
 "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
 "ctx/language": "en",
 "ctx/territory": "GB",
 "ctx/time": "2015-10-12T00:11:55.518+02:00",
"procedures_list/procedures/procedure:0/ism_transition/current_state|value": "completed",
"procedures_list/procedures/procedure:0/ism_transition/careflow_step|code": "at0043",
"procedures_list/procedures/procedure:0/procedure_name|value": "total replacement of hip",
"procedures_list/procedures/procedure:0/procedure_name|terminology": "SNOMED-CT",
"procedures_list/procedures/procedure:0/procedure_name|code": "52734007",
"procedures_list/procedures/procedure:0/procedure_notes": "No intra-operative problems",
"procedures_list/procedures/procedure:0/_other_participation:0|name": "Jo Douglas",
"procedures_list/procedures/procedure:0/_other_participation:0|function": "performer",
"procedures_list/procedures/procedure:0/time": "2015-07-15T15:11:33.829Z"
}
```

#### Notes:

None
