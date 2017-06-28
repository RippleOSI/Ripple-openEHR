#### Patient Headings Dashboard


#### Version:

1.0.1

8-Jun-2017

#### TemplateID:
`Ripple Dashboard Cache.v1`

#### Summary AQL /query:

To populate the list of per-Heading composition counts and most recent date.

```
select
  e/ehr_id/value as ehrId,
  a/context/other_context[at0001]/items[at0005]/value/magnitude as diagnosesCount,
  a/context/other_context[at0001]/items[at0008]/value/value as diagnosesDate,
  a/context/other_context[at0001]/items[at0002]/value/magnitude as ordersCount,
  a/context/other_context[at0001]/items[at0006]/value/value as ordersDate,
  a/context/other_context[at0001]/items[at0004]/value/magnitude as resultsCount,
  a/context/other_context[at0001]/items[at0009]/value/value as resultsDate,
  a/context/other_context[at0001]/items[at0003]/value/magnitude as vitalsCount,
  a/context/other_context[at0001]/items[at0007]/value/value as vitalsDate
from EHR e
contains COMPOSITION a[openEHR-EHR-COMPOSITION.ripple_cache.v0]
where e/ehr_id/value = '{{ehrId}}'
```


#### Sample Composition(FLAT JSON) for PUT /composition:

To update  composition for a single item via the /composition Ehrscape API call.

It is suggested that this persistent-style composition is originally created as a POST as part of initial provisioning of the domain.

It will then be updated as a PUT /composition i.e. overwrite, whenever a new composition within the tracked categories is created.


```
{
    "ctx/language": "en",
    "ctx/territory": "GB",
    "ctx/composer_name": "Ripple system",
    "ctx/time": "2017-04-21T18:56:55.830Z",
    "ctx/id_namespace": "HOSPITAL-NS",
    "ctx/id_scheme": "HOSPITAL-NS",
    "ctx/health_care_facility|name": "Ripple",
    "ctx/health_care_facility|id": "9091",
    "ripple_dashboard_cache/context/orders": 200,
    "ripple_dashboard_cache/context/orders_date": "2017-04-21T18:56:55.831Z",
    "ripple_dashboard_cache/context/vitals": 1,
    "ripple_dashboard_cache/context/vitals_date": "2017-04-21T18:56:55.831Z",
    "ripple_dashboard_cache/context/results": 56,
    "ripple_dashboard_cache/context/results_date": "2017-04-21T18:56:55.831Z",
    "ripple_dashboard_cache/context/diagnoses": 5,
    "ripple_dashboard_cache/context/diagnoses_date": "2017-04-21T18:56:55.831Z"
}
```
