#### Heading

### MDT Reports

#### Version:

1.3.2 - 24-Jan-2018
Removed incorrect at0002 node predicate in ism_transition in AQL.

1.3.1 - 23-Jan-2018
Corrected AQL error

1.3.0 - 22-Jan-2018
Added marked up FLAT JSON example.

1.2.1 - 19-June-2017
Restored FLAT JSON example, moved AQL to correct place.

1.2.0 - 15-Jun-2017
Corrected AQL to work with updated template (uses ACTION.service.v0)

#### TemplateID:
`IDCR - Minimal MDT Output Report.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    e/ehr_status/subject/external_ref/id/value as subjectId,
    e/ehr_status/subject/external_ref/namespace as subjectNamespace,
    a/composer/name as authorName,
    a/composer/external_ref/id/value as authorIdentifier,
    a/composer/external_ref/namespace as authorNamespace,
    a/context/start_time/value as meeting_date,
    b_a/time/value as request_date,
    a_a/protocol[at0008]/items[at0011]/value/value as service_team,
    a_b/data[at0001]/items[at0004]/value/value as question,
    a_c/data[at0001]/items[at0002]/value/value as notes,
    b_a/ism_transition/careflow_step/defining_code/code_string as careflow_step,
    b_a/protocol[at0015]/items[at0018]/value/value as Service_team
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.report.v1]
contains (
    INSTRUCTION a_a[openEHR-EHR-INSTRUCTION.request.v0] or
    EVALUATION a_b[openEHR-EHR-EVALUATION.reason_for_encounter.v1] or
    EVALUATION a_c[openEHR-EHR-EVALUATION.recommendation.v1] or
    ACTION b_a[openEHR-EHR-ACTION.service.v0])
where
    a/name/value='MDT Output Report' and
    b_a/ism_transition/careflow_step/defining_code/code_string='at0026'
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
  "mdt_output_report/referral_details/mdt_referral/request:0/service_name": "MDT referral",
  "mdt_output_report/referral_details/mdt_referral/request:0/_uid: "{{guid}}",
  "mdt_output_report/referral_details/mdt_referral/request:0/timing": "R2/2016-12-19T18:00:00Z/P2M",
  "mdt_output_report/referral_details/mdt_referral/narrative": "MDT request",
  "mdt_output_report/referral_details/mdt_referral/service_team": "MDT Prostate Cancer team",
  "mdt_output_report/referral_details/referral_requested/ism_transition/current_state|code": "526",
  "mdt_output_report/referral_details/referral_requested/ism_transition/careflow_step|code": "at0026",
  "mdt_output_report/referral_details/referral_requested/service_requested": "MDT Referral",
  "mdt_output_report/referral_details/referral_requested/time": "2016-12-19T18:03:13.395Z",
  "mdt_output_report/history/question_to_mdt/question_to_mdt": "Increasing back pain",
  "mdt_output_report/plan_and_requested_actions/recommendation/meeting_discussion": "Investigations normal. Review in 3 weeks"
}
```

**Marked up FLAT JSON example**
```
{
  "ctx/composer_name": "Dr Tony Shannon", //author
  "ctx/health_care_facility|id": "999999-345", //boilerplate
  "ctx/health_care_facility|name": "Home", //boilerplate
  "ctx/id_namespace": "NHS-UK", //boilerplate
  "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3", //boilerplate
  "ctx/language": "en", // boilerplate
  "ctx/territory": "GB", // boilerplate
  "ctx/time": "2016-02-24T00:11:02.518+02:00", //meetingDate
  "mdt_output_report/referral_details/mdt_referral/request:0/_uid: "{{guid}}",
  "mdt_output_report/referral_details/mdt_referral/request:0/service_name": "MDT referral", //Boilerplate
  "mdt_output_report/referral_details/mdt_referral/request:0/timing": "R2/2016-12-19T18:00:00Z/P2M", //Boilerplate
  "mdt_output_report/referral_details/mdt_referral/narrative": "MDT request", //boilerplate
  "mdt_output_report/referral_details/mdt_referral/service_team": "MDT Prostate Cancer team", //service_team
  "mdt_output_report/referral_details/referral_requested/ism_transition/current_state|code": "526", //boilerplate
  "mdt_output_report/referral_details/referral_requested/ism_transition/careflow_step|code": "at0026",//boilerplate
  "mdt_output_report/referral_details/referral_requested/service_requested": "MDT Referral", //Boilerplate
  "mdt_output_report/referral_details/referral_requested/time": "2016-12-19T18:03:13.395Z", //request_date
  "mdt_output_report/history/question_to_mdt/question_to_mdt": "Increasing back pain", //question
  "mdt_output_report/plan_and_requested_actions/recommendation/meeting_discussion": "Investigations normal. Review in 3 weeks" //notes
}
```




#### Notes:
