#### Heading

### Immunisations

#### Version:

1.0.0

20-Dec-2016

#### TemplateID:
`IDCR - Immunisation summary.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    b_a/description[at0001]/items[at0002]/value/value as vaccination_name,
    b_a/description[at0001]/items[at0004]/value/magnitude as series_number,
    b_a/description[at0001]/items[at0015]/value/value as comment,
    b_a/time/value as vaccination_time
from EHR e[ehr_id/value='{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.health_summary.v1]
contains ACTION b_a[openEHR-EHR-ACTION.immunisation_procedure.v1]
where a/name/value='Immunisation summary'
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
"ctx/health_care_facility|name": "Ripple View Care Home",
"ctx/id_namespace": "NHS-UK",
"ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
"ctx/language": "en",
"ctx/territory": "GB",
"ctx/time": "2016-02-24T00:11:02.518+02:00", //Date
"immunisation_summary/immunisation_procedure:0/ism_transition/current_state|code": "532", //Hardwired
"immunisation_summary/immunisation_procedure:0/immunisation_name": "Hepatitis B",
"immunisation_summary/immunisation_procedure:0/series_number": 3,
"immunisation_summary/immunisation_procedure:0/comment": "Immunisation complete",
"immunisation_summary/immunisation_procedure:0/time": "2016-12-20T07:45:20.622Z" //Vaccination time
}

```

#### Notes:

None
