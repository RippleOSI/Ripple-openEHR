#### Heading

### Clinical Notes

#### Version:

1.0.1

19-Dec-2016

#### TemplateID:
`RIPPLE - Clinical Note.v1`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    b_a/data[at0001]/items[at0002]/value/value as note,
    b_a/name/value as type
from EHR e[ehr_id/value='{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.encounter.v1]
contains EVALUATION b_a[openEHR-EHR-EVALUATION.clinical_synopsis.v1]
where a/name/value='Clinical Notes'
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
  "clinical_notes/clinical_synopsis:0/_name|value": "SOAP Note",
   "clinical_notes/clinical_synopsis:0/notes": "S: Patient was very concerned about her welfare, no specific complaints, but some mild symptoms of depression (poor sleep, low energy levels etc)\nO: Comfortable: Vitals Normal BP 120/80; \nA; Social concerns and mild anxiety\nP; refer to Mental Health Team & Social Services for better support. DC with sister today."
}
```

#### Notes:
None
