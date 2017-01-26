#### Heading

### Referrals

#### Version:

1.1.0

18-Dec-2016

#### TemplateID:
`IDCR - Service Request.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.
# **** Not currently valid ****

```
select
    a/composer/name as author,
    a/uid/value as uid,
    a/context/start_time/value as date_created,
    b_a/activities[at0001]/description[at0009]/items[at0121]/value/value as referral_to,
    b_a/activities[at0001]/description[at0009]/items[at0062]/value/value as referral_reason,
    b_a/activities[at0001]/description[at0009]/items[at0064]/value/value as clinical_summary,
    b_a/protocol[at0008]/items[openEHR-EHR-CLUSTER.individual_person_uk.v1, 'Requestor']/items[openEHR-EHR-CLUSTER.person_name.v1]/items[at0001]/value/value as referralFrom,
    b_a/protocol[at0008]/items[openEHR-EHR-CLUSTER.organisation.v1, 'Receiver']/items[at0001]/value/value as referralTo,
    b_a/protocol[at0008]/items[at0011]/value/value as referral_ref,
    b_c/description[at0001]/items[at0011]/value/value as Service_Service_name,
    b_c/description[at0001]/items[at0028]/value/value as Outcome,
    b_c/time/value as dateOfState,
    b_c/ism_transition/current_state/value as state,
    b_c/ism_transition/current_state/defining_code/code_string as stateCode,
    b_c/ism_transition/careflow_step/value as careflow,
    b_c/ism_transition/careflow_step/defining_code/code_string as careflowCode
 from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.request.v1]
contains
  (INSTRUCTION b_a[openEHR-EHR-INSTRUCTION.request.v0]
   and
   ACTION b_c[openEHR-EHR-ACTION.service.v0])
where b_c/ism_transition/current_state/defining_code/code_string = '526'```

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
  "ctx/time": "2016-03-24T00:11:02.518+02:00",
    "request_for_service/referral_details/service_request:0/request:0/service_name": "Optometry",
    "request_for_service/referral_details/service_request:0/request:0/reason_for_request": "Deteriorating vision",
    "request_for_service/referral_details/service_request:0/request:0/reason_description": "Long-standing impaired visual acuity - getting much worse",
    "request_for_service/referral_details/service_request:0/request:0/timing": "R5/2016-10-04T21:00:00Z/P1M",
    "request_for_service/referral_details/service_request:0/request:0/timing|formalism": "timing",
    "request_for_service/referral_details/service_request:0/requestor/person_name/unstructured_name": "Tony Shannon",
    "request_for_service/referral_details/service_request:0/receiver_identifier": "3f74c2e6-2ae0-4680-b8b2-c6daec8f9309",
    "request_for_service/referral_details/service_request:0/receiver/name_of_organisation": "Ripplefields Optometry service",
    "request_for_service/referral_details/service_request:0/narrative": "Optometry",
 	  "request_for_service/referral_details/service:0/receiver_identifier": "3f74c2e6-2ae0-4680-b8b2-c6daec8f9309",
    "request_for_service/referral_details/service:0/ism_transition/current_state|code": "526",
    "request_for_service/referral_details/service:0/ism_transition/careflow_step|code": "at0026",
    "request_for_service/referral_details/service:0/service_name": "Optometry"
  }
```

#### Notes:

None
