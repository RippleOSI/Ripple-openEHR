#### Heading

### Problems

#### Version:

1.0.0

18-Dec-2016

#### TemplateID:
`IDCR - Problem List.v1`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
  a/uid/value as uid,
  a/composer/name as author,
  a/context/start_time/value as date_created,
  b_a/data[at0001]/items[at0002]/value/value as problem,
  b_a/data[at0001]/items[at0002]/value/defining_code/code_string as problem_code,
  b_a/data[at0001]/items[at0002]/value/defining_code/terminology_id/value as problem_terminology,
  b_a/data[at0001]/items[at0009]/value/value as description,
  b_a/data[at0001]/items[at0077]/value/value as onset_date,
  b_a/provider/external_ref/id/value as originalComposition,
  b_a/provider/name as originalSource
 from EHR e [ehr_id/value = '{{ehrId}}']
 contains COMPOSITION a[openEHR-EHR-COMPOSITION.problem_list.v1]
 contains EVALUATION b_a[openEHR-EHR-EVALUATION.problem_diagnosis.v1]
 where a/name/value='Problem list'
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
		"ctx/composer_id": "G33567",
    "ctx/health_care_facility|id": "999999-345",
    "ctx/health_care_facility|name": "Ripple Valley Hospital NHS",
    "ctx/id_namespace": "NHS-UK",
    "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
    "ctx/language": "en",
    "ctx/territory": "GB",
    "ctx/time": "2015-10-22T00:11:02.518+02:00",
    "problem_list/problems_and_issues:0/problem_diagnosis:0/_provider|name": "iHEALTHLINK Discharge",
    "problem_list/problems_and_issues:0/problem_diagnosis:0/_provider|id": "http://showcase.1.rippleosi.org/1234567",
    "problem_list/problems_and_issues:0/problem_diagnosis:0/date_time_of_onset": "2000-07-17T12:30:37.553Z",
    "problem_list/problems_and_issues:0/problem_diagnosis:0/problem_diagnosis_name|code": "299757012",
    "problem_list/problems_and_issues:0/problem_diagnosis:0/problem_diagnosis_name|terminology": "SNOMED-CT",
    "problem_list/problems_and_issues:0/problem_diagnosis:0/problem_diagnosis_name|value": "angina pectoris"    
}

```

#### Notes:

None
