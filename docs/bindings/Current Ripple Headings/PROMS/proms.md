#### Heading

### PROMS

#### Version:

1.2.0: 16 Mar 2018 - Added support for linkage to Procedure List
1.1.0: 27 Feb 2018 - Fixed incorrect EHR clause in AQL
1.0.0: 23 Feb 2018

#### TemplateID:
`Ripple PROMS.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    a/context/other_context[at0001]/items[at0002]/value/value as PROMS_type,
    a/context/other_context[at0001]/items[at0005]/value/value as Procedure_link,
    b_c/description[at0001]/items[at0002]/value/value as Procedure_value,
    b_c/description[at0001]/items[at0002]/value/defining_code/code_string as Procedure_code,
    b_c/description[at0001]/items[at0002]/value/defining_code/terminology_id as Procedure_terminology,
    b_a/items[at0001]/value/magnitude as Pain_scale,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/symbol/defining_code/code_string as a_1_Health_in_general,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/symbol/defining_code/code_string as a_2_Health_compared_to_1_year_ago,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0029]/items[at0020]/value/symbol/defining_code/code_string as a_3_Vigorous_activies,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0029]/items[at0024]/value/symbol/defining_code/code_string as a_4_Moderate_activies,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0029]/items[at0028]/value/symbol/defining_code/code_string as a_5_Lifting_or_carrying_groceries
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.report.v1]
contains (
    CLUSTER b_a[openEHR-EHR-CLUSTER.pain_vas.v0] or
    OBSERVATION b_b[openEHR-EHR-OBSERVATION.sf36.v0] or
    ACTION b_c[openEHR-EHR-ACTION.procedure.v1])
where a/name/value='Generic PROMS'
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
"ctx/time": "2017-11-14T00:11:02.518+02:00",
   "generic_proms/context/proms_type": "Foot and ankle PROMS - pre-op assessment",
   "generic_proms/context/ecis_procedure_link": "ehr:///3cd3bef6-d180-4d73-91c7-fb4ea231bb1b::hcbox.oprn.ehrscape.com::1/content/items[uid/value='140413da-b1d4-41c4-a3f3-d86abe47a9f1']",
   "generic_proms/procedure/time" : "2017-11-14T00:11:02.518+02:00",
   "generic_proms/procedure/ism_transition/current_state|code": "245",
   "generic_proms/procedure/ism_transition/careflow_step|code": "at0047",
   "generic_proms/procedure/procedure_name|value": "Excision of cataract",
   "generic_proms/procedure/procedure_name|terminology": "SNOMED-CT",
   "generic_proms/procedure/procedure_name|code": "54885007",
   "generic_proms/sf-36:0/a1._health_in_general|code": "at0009",
   "generic_proms/sf-36:0/a2._health_compared_to_1_year_ago|code": "at0017",
   "generic_proms/sf-36:0/limitations_to_activities_during_typical_day/a3._vigorous_activies|code": "at0022",
   "generic_proms/sf-36:0/limitations_to_activities_during_typical_day/a4._moderate_activies|code": "at0027",
   "generic_proms/sf-36:0/limitations_to_activities_during_typical_day/a5._lifting_or_carrying_groceries|code": "at0030",
   "generic_proms/pain_vas/pain_vas:0/pain_scale": 81
}

```



#### Template Constraints

**generic_proms/context/proms_type : Text**

Suggest this is just populated as text from a dropdown list e.g.

"Foot and ankle PROMS: pre-op assessment"
"Foot and ankle PROMS: 6-week assessment"
"Foot and ankle PROMS: 12-week assessment"
"Rheumatology: initial assessment"
"Rheumatology: quarterly review"
"Rheumatology: annual review"


**generic_proms/context/ecis_procedure_link: Text**

This is the link to the original procedure and is derived from the compositionID of the original compositionUId and the entryUid of the individual procedure Entry within that composition, (which will need to be created when the procedure composition is  saved - see notes below.

The link string needs to be in the form ...

```
"ehr:///{{compositionUid}}/content/items[uid/value='{{entryUid}}']"
```

Example...
``` "ehr:///3cd3bef6-d180-4d73-91c7-fb4ea231bb1b::hcbox.oprn.ehrscape.com::1/content/items[uid/value='140413da-b1d4-41c4-a3f3-d86abe47a9f1']",
```
The information in the link will allow the original procedure to be

**generic_proms/pain_vas/pain_vas:0/pain_scale : Integer**

0-100
Pain Score : 0-100
Note that the UI uses 0-10 rather than 0-100 suggest use 0-100 in UI.

**generic_proms/sf-36:0/a1._health_in_general|code : Coded_text**

"1. In general, would you say your health is:"

at0009:Excellent
at0010:Very good
at0011:Good
at0012:Fair
at0013:Poor

**generic_proms/sf-36:0/a2._health_compared_to_1_year_ago|code : Coded_text**

"2. Compared to one year ago, how would you rate your health in general now?"

Valid codes:

at0015:Much better
at0016:Somewhat better
at0017:About the same§
at0018:Somewhat worse
at0019:Much worse

**generic_proms/sf-36:0/limitations_to_activities_during_typical_day/a3._vigorous_activies|code**

"3. Does your health now limit you in vigorous activities?"

Valid codes:

at0021:Yes,limited a lot
at0022:Yes,limited a little
at0023:No, not limited at all

**generic_proms/sf-36:0/limitations_to_activities_during_typical_day/a4._moderate_activies|code**

"4. Does your health now limit you in moderate activities?"

Valid codes:

at0025:Yes,limited a lot
at0026:Yes,limited a little
at0027:No, not limited at all

**generic_proms/sf-36:0/limitations_to_activities_during_typical_day/a5._lifting_or_carrying_groceries|code**

"5. Does your health now limit you in lifting or carrying groceries?"

Valid codes:

at0030:Yes,limited a lot
at0031:Yes,limited a little
at0032:No, not limited at all


#### Notes on linkages

1.  We need to start routinely adding 'entry level uid' to the Procedures (and actually for all the lists, allergies, procedures). This supports the linkage requirement but also is need for FHIR alignment.

This is easy to do e.g. for Procedures.

add this line to each composition commit.

` "procedures_list/procedures/procedure:0/_uid": "{{$guid}}", `

2. To pull up the list of candidate Procedures from the Procedure List we need to run this AQL ...

```
select  a/uid/value as compositionUid,
  b_a/uid/value as entryUid,
  b_a/description[at0001]/items[at0002] as Procedure_name,
  b_a/time/value as Procedure_time,
  b_a/ism_transition/current_state/defining_code/code_string as Status_code,    b_a/ism_transition/careflow_step/defining_code/code_string as Careflow_step_code   
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.health_summary.v1]
contains ACTION b_a[openEHR-EHR-ACTION.procedure.v1]
where a/name/value='Procedures list'
order by b_a/time/value desc
```

when the procedure is selected we need to construct the linkString from these values

```
"ehr:///{{compositionUid}}/content/items[uid/value='{{entryUid}}']"
```

if `entryUid` is `null` then do

```
"ehr:///{{compositionUid}}/content/items"
```

3. There is a good argument for recording a local copy of the Procedure inside this composition.

a) It is more performant for future display as the link does not need to be resolved each time.
b) It can protect against deletions changes in the original procedure. Handling re-versioned links is a real nightmare.

So populate the local in-line Procedure from the AQL as follows...

```
"generic_proms/procedure/time" = "{Procedure_time}}"
"generic_proms/procedure/ism_transition/current_state|code" = "{{Status_code}}"
"generic_proms/procedure/ism_transition/careflow_step|code" = "{{Careflow_step_code}}"
"generic_proms/procedure/procedure_name|value": "{{Procedure_value}}"
"generic_proms/procedure/procedure_name|terminology": "{{Procedure_terminology}}",
"generic_proms/procedure/procedure_name|code": "{{Procedure_code}}"  
```
