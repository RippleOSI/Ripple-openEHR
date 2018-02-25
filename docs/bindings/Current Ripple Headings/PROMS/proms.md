#### Heading

### PROMS

#### Version:

1.0.0

23 Feb 2018

#### TemplateID:
`Ripple PROMS.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    b_c/description[at0001]/items[at0002]/value/value as Procedure_value,
    b_c/description[at0001]/items[at0002]/value/defining_code/code_string as Procedure_code,
    b_c/description[at0001]/items[at0002]/value/defining_code/terminology_id as Procedure_terminology,
    b_a/items[at0001]/value/magnitude as Pain_scale_magnitude,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/symbol/defining_code/code_string as a_1_Health_in_general,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/symbol/defining_code/code_string as a_2_Health_compared_to_1_year_ago,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0029]/items[at0020]/value/symbol/defining_code/code_string as a_3_Vigorous_activies,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0029]/items[at0024]/value/symbol/defining_code/code_string as a_4_Moderate_activies,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0029]/items[at0028]/value/symbol/defining_code/code_string as a_5_Lifting_or_carrying_groceries
from EHR e
contains COMPOSITION a[openEHR-EHR-COMPOSITION.report.v1]
contains (
    CLUSTER b_a[openEHR-EHR-CLUSTER.pain_vas.v0] or
    OBSERVATION b_b[openEHR-EHR-OBSERVATION.sf36.v0] or
    ACTION b_c[openEHR-EHR-ACTION.procedure.v1])
where a/name/value='PROMs'
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
"ctx/time": "2017-01-14T00:11:02.518+02:00",
   //FIXED BOILERPLATE
   "proms/procedure:0/ism_transition/current_state|code": "245",
   //FIXED BOILERPLATE
   "proms/procedure:0/ism_transition/careflow_step|code": "at0047",
   "proms/procedure:0/procedure_name|value": "total replacement of hip",
   "proms/procedure:0/procedure_name|terminology": "SNOMED-CT",
    "proms/sf-36:0/a1._health_in_general|code": "at0011",
    "proms/sf-36:0/a2._health_compared_to_1_year_ago|code": "at0017",
    "proms/sf-36:0/limitations_to_activities_during_typical_day/a3._vigorous_activies|code": "at0021",
    "proms/sf-36:0/limitations_to_activities_during_typical_day/a4._moderate_activies|code": "at0027",
    "proms/sf-36:0/limitations_to_activities_during_typical_day/a5._lifting_or_carrying_groceries|code": "at0031",
    "proms/pain_vas/pain_vas:0/pain_scale": 44
}

```



#### Template Constraints

Score : 0-100

**proms/pain_vas/pain_vas:0/pain_scale : Integer**
0-100

**proms/sf-36:0/a1._health_in_general|code : Coded_text**

"1. In general, would you say your health is:"

at0009:Excellent
at0010:Very good
at0011:Good
at0012:Fair
at0013:Poor

**proms/sf-36:0/a2._health_compared_to_1_year_ago|code : Coded_text**
"2. Compared to one year ago, how would you rate your health in general now?"

at0015:Much better
at0016:Somewhat better
at0017:About the same§
at0018:Somewhat worse
at0019:Much worse

**proms/sf-36:0/limitations_to_activities_during_typical_day/a3._vigorous_activies|code**
"3. Does your health now limit you in vigorous activities?"

at0021:Yes,limited a lot
at0022:Yes,limited a little
at0023:No, not limited at all

**proms/sf-36:0/limitations_to_activities_during_typical_day/a4._moderate_activies|code**
"4. Does your health now limit you in moderate activities?"
at0025:Yes,limited a lot
at0026:Yes,limited a little
at0027:No, not limited at all

**proms/sf-36:0/limitations_to_activities_during_typical_day/a5._lifting_or_carrying_groceries|code**
"5. Does your health now limit you in lifting or carrying groceries?"
at0030:Yes,limited a lot
at0031:Yes,limited a little
at0032:No, not limited at all


#### Notes:
None
