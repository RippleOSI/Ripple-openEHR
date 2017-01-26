#### Heading

### Lab Results

#### Version:

1.0.0

20-Dec-2016

#### TemplateID:
`IDCR - Laboratory Test Report.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
a/uid/value as uid,
a/composer/name as author,
a/context/start_time/value as date_created,
    a_b/data[at0001]/origin/value as sample_time,
    a_b/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/value as test_name,
    a_b/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/defining_code/code_string as test_name_code,
    a_b/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/defining_code/terminology_id/value as test_name_terminology,
    a_b/data[at0001]/events[at0002]/data[at0003]/items[at0073]/value/value as status,
    a_b/data[at0001]/events[at0002]/data[at0003]/items[at0057]/value/value as conclusion,
    a_a/items[at0002]/name/value as Laboratory_result_header,
    a_a/items[at0002]/items[at0001]/name/value as result_name,
    a_a/items[at0002]/items[at0001]/name/defining_code/code_string as result_name_code,
    a_a/items[at0002]/items[at0001]/name/defining_code/terminology_id/value as result_name_terminology,
    a_a/items[at0002]/items[at0001]/value/magnitude as result_magnitude,
    a_a/items[at0002]/items[at0001]/value/units as result_units,
    a_a/items[at0002]/items[at0001]/value/normal_range/lower/magnitude as normal_range_lower,
        a_a/items[at0002]/items[at0001]/value/normal_range/lower/units as normal_range_lower_units,
    a_a/items[at0002]/items[at0001]/value/normal_range/upper/magnitude as normal_range_upper,
        a_a/items[at0002]/items[at0001]/value/normal_range/upper/units as normal_range_upper_units,
    a_a/items[at0002]/items[at0001]/value/normal_range/lower_included as lower_included,
    a_a/items[at0002]/items[at0001]/value/normal_range/upper_included as upper_included,
    a_a/items[at0002]/items[at0001]/value/normal_range/lower_unbounded as lower_unbounded,
    a_a/items[at0002]/items[at0001]/value/normal_range/upper_unbounded as upper_unbounded
from EHR e[ehr_id/value='{{ehrId}}']
contains COMPOSITION a
contains
    OBSERVATION a_b[openEHR-EHR-OBSERVATION.laboratory_test.v0]
contains
    CLUSTER a_a[openEHR-EHR-CLUSTER.laboratory_test_panel.v0]
```

Alternative AQL using Resultset Objects (does not work on EtherCis)
```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    a_b as lab_test
from EHR e[ehr_id/value='{{ehrId}}']
contains COMPOSITION a
contains OBSERVATION a_b[openEHR-EHR-OBSERVATION.laboratory_test.v0]
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
 "ctx/composer_name": "Dr Lab",
 "ctx/health_care_facility|id": "999999-34567",
 "ctx/health_care_facility|name": "Super Labs Inc",
 "ctx/id_namespace": "NHS-UK",
 "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
 "ctx/language": "en",
 "ctx/territory": "GB",
 "ctx/time": "2015-04-15T00:11:02.518+02:00",
 "ctx/history_origin": "2015-04-10T00:11:02.518+02:00",
 "laboratory_test_report/laboratory_test/test_name|value": "Urea, electrolytes and creatinine measurement",
 "laboratory_test_report/laboratory_test/test_name|code": "444164000",
 "laboratory_test_report/laboratory_test/test_name|terminology": "SNOMED-CT",
 "laboratory_test_report/laboratory_test/test_status|code": "at0038",
 "laboratory_test_report/laboratory_test/test_status|value": "Final",
 "laboratory_test_report/laboratory_test/test_status_timestamp": "2015-04-10T00:11:02.518+02:00",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_name|value": "Urea",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_name|code": "365755003",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_name|terminology": "SNOMED-CT",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value|magnitude": 3.6,
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_normal_range/lower|magnitude": "2.5",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_normal_range/lower|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_normal_range/upper|magnitude": "6.6",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/result_value/_normal_range/upper|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:0/comment": "this is free text",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_name|value": "Creatinine",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_name|code": "70901006",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_name|terminology": "SNOMED-CT",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value|magnitude": 97,
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_normal_range/lower|magnitude": "80",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_normal_range/lower|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_normal_range/upper|magnitude": "110",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:1/result_value/_normal_range/upper|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_name|value": "Sodium",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_name|code": "365761000",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_name|terminology": "SNOMED-CT",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value|magnitude": 122,
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_normal_range/lower|magnitude": "133",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_normal_range/lower|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_normal_range/upper|magnitude": "146",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:2/result_value/_normal_range/upper|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_name|value": "Potassium",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_name|code": "365760004",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_name|terminology": "SNOMED-CT",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value|magnitude": "6.1",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_normal_range/lower|magnitude": "3.5",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_normal_range/lower|unit": "mmol/l",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_normal_range/upper|magnitude": "5.3",
 "laboratory_test_report/laboratory_test/laboratory_test_panel/laboratory_result:3/result_value/_normal_range/upper|unit": "mmol/l"
}
```

#### Notes:

None
