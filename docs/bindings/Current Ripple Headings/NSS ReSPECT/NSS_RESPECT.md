#### Heading

### NHS Scotland ReSPECT Form

#### Version:

1.0.0

4-Mar-2019

#### TemplateID:
RESPECT_NSS-v0


#### Retrieve ALL versions of Respect composition:

To retrieve All versions of the Respect composition for the single patoent with a given ehrId.

```
select
  b/uid as version_uid,
  b/composer/name as authorName,
  b/context/start_time/value as dateUpdated
from EHR e[ehr_id/value = {{ehrId}}] contains
  VERSIONED_OBJECT a contains
  VERSION v[all_versions]
  contains COMPOSITION b
  where b/name/value = 'NSS RESPECT Form'
  ORDER BY b/context/start_time/value DESC
```

#### Retrieve current version of Respect composition:

To retrieve the active version of the Respect composition. Select the first item (should only be one in a production system).

```
select
  b/uid/value as compositionUid,
  b/composer/name as authorName,
  b/context/start_time/value as dateUpdated
from EHR e[ehr_id/value = {{ehrId}}] contains
  VERSIONED_OBJECT a contains
  VERSION v[all_versions]
  contains COMPOSITION b
  where b/name/value = 'NSS RESPECT Form'
  ORDER BY b/context/start_time/value DESC
```

#### Retreive composition detail:
To populate the detailed view / edit when a single record within the heading is selected.

Suggest using the /composition GET cal which will retrieve the whole composition in a similar format to that committed.

```
curl -X GET '{{base_url}}/rest/v1/composition/{{uid}}?format=FLAT'
```

#### Sample Composition(FLAT JSON) for POST/PUT /composition:

To create or update a composition for a single item via the /composition Ehrscape API call.

After the first commit, every subsequent commit should be a PUT - so that the original composition is continually updated.





```json
{


      //BOILERPLATE
      "nss_respect_form/language|code": "en",
      //BOILERPLATE
      "nss_respect_form/language|terminology": "ISO_639-1",
      //BOILERPLATE
      "nss_respect_form/territory|code": "GB",
      //BOILERPLATE
      "nss_respect_form/territory|terminology": "ISO_3166-1",

      // Hospital / GP surgery identifier
      "nss_respect_form/context/_health_care_facility|id": "FV-DGH",
      //BOILERPLATE
       "nss_respect_form/context/_health_care_facility|id_scheme": "NHSScotland",
      //BOILERPLATE
       "nss_respect_form/context/_health_care_facility|id_namespace": "NHSScotland",
       "nss_respect_form/context/_health_care_facility|name": "Forth Valley DGH",
      //Date created /updated
         "nss_respect_form/context/start_time": "2016-12-20T00:11:02.518+02:00",
       //BOILERPLATE
       "nss_respect_form/context/setting|code": "238",
       //BOILERPLATE     
       "nss_respect_form/context/setting|value": "other care",
       //BOILERPLATE     
       "nss_respect_form/context/setting|terminology": "openehr",

       //Author name
       "nss_respect_form/composer|name": "Dr Jonty Shannon",
       //Author ID
       "nss_respect_form/composer|id": "12345",
       //BOILERPLATE            
       "nss_respect_form/composer|id_scheme": "NHSScotland",
       //BOILERPLATE     
       "nss_respect_form/composer|id_namespace": "NHSScotland",

     //Form status
    //Valueset: Started; Incomplete ; Complete and signed
    "nss_respect_form/context/status": "Complete and signed",

    //2. Summary of relevant information
    "nss_respect_form/respect_headings/a2._summary_of_relevant_information/a2.0_relevant_information/respect_summary/narrative_summary": "Lung cancer with bone metastases",

    "nss_respect_form/respect_headings/a2._summary_of_relevant_information/a2.3_other_relevant_planning_documents/advance_planning_documentation/advance_planning_document:0/name": "planning document",
    "nss_respect_form/respect_headings/a2._summary_of_relevant_information/a2.3_other_relevant_planning_documents/advance_planning_documentation/advance_planning_document:0/location": "Patient's home",
    "nss_respect_form/respect_headings/a2._summary_of_relevant_information/a2.3_other_relevant_planning_documents/advance_planning_documentation/advance_planning_document:0/link_to_document:0/link_to_document": "http://med.tube.com/sample",
    "nss_respect_form/respect_headings/a2._summary_of_relevant_information/a2.3_other_relevant_planning_documents/advance_planning_documentation/advance_planning_document:0/link_to_document:0/link_to_document|mediatype": "application/pdf",
    // I am not sure if this is mandatory
    "nss_respect_form/respect_headings/a2._summary_of_relevant_information/a2.3_other_relevant_planning_documents/advance_planning_documentation/advance_planning_document:0/link_to_document:0/link_to_document|size": 504903212,


    // 3. Care Priority scale - current bug DO NOT POPULATE
    // This is not committing correctly, possibly due to a bug with the Marand .opt generator
    // "nss_respect_form/respect_headings/a3._personal_preferences/preferred_priorities_of_care/care_priority_scale": 17,

    // 3. What is most important to you?
    "nss_respect_form/respect_headings/a3._personal_preferences/preferred_priorities_of_care/patient_care_priority": "Patient care priority 99",

    // 4. Clinical focus - radio button valueset : Symptom control;Life-sustaining treatment
    "nss_respect_form/respect_headings/a4._clinical_recommendations/recommendation/clinical_focus": "Symptom control",
  // 4. Clinical guidance
    "nss_respect_form/respect_headings/a4._clinical_recommendations/recommendation/clinical_guidance_on_interventions": "Clinical guidance on interventions 32",

    // 4. CPR decision
    // Valueset:
      //  at0004 | CPR attempts recommended adult or child	Cardio-pulmonary resuscitation is recommended for adult or child.
      //  at0005 | CPR attempts not recommended adult or child.	Cardiopulmonary resuscitation is not recommended for adult or child.
      //  at0027 | For modified CPR child only	Modified CPR is recommended for child only.
    "nss_respect_form/respect_headings/a4._clinical_recommendations/cpr_decision/cpr_decision|code": "at0027",
    // 4. Date of CPR decision
    "nss_respect_form/respect_headings/a4._clinical_recommendations/cpr_decision/date_of_cpr_decision": "2019-03-03T23:09:53.12+01:00",

    // 5. Does the patient have capacity? Yes = true No = false
    "nss_respect_form/respect_headings/a5._capacity_and_representation/capacity_respect/sufficient_capacity": false,

    // 5. Do they have a legal proxy?
    // Valueset:
    // at0004 | yes
    // at0005 | no
    // at0006 | Unknown
    "nss_respect_form/respect_headings/a5._capacity_and_representation/capacity_respect/legal_proxy|code": "at0005",

    // 6. Involvement in Recommendations
    // Valueset:
    // at0003 | A Person has mental capacity
    // at0004 | B Person does not have mental capacity
    // at0005 | C1 Person less than 18 or 16 with sufficient maturity
    // at0011 | C2 Person less than 18 or 16 without sufficient maturity
    // at0012 | C3 Person less than 18 or 16 parental decision
    // at0013 | D No other option selected
    "nss_respect_form/respect_headings/a6._involvement_in_making_plan/involvement_respect/involvement_in_recommendations/involvement|code": "at0003",
    //6. Reason for not selecting A or B or C
    "nss_respect_form/respect_headings/a6._involvement_in_making_plan/involvement_respect/involvement_in_recommendations/reason_for_not_selecting_options_a_or_b_or_c": "Reason for not selecting Options A or B or C 88",

    //6. Details of those involved in decisio making and location of documents
    "nss_respect_form/respect_headings/a6._involvement_in_making_plan/involvement_respect/name_and_role_of_those_involved_in_decision_making": "Name and role of those involved in decision making 57",
    //BOILERPLATE
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/ism_transition/current_state|value": "completed",

    // 7.Clinician Signatures
    // Valueset:
    // ReSPECT clinician signature
    //ReSPECT Senior Responsible Clinician signature

    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/service_name": "ReSPECT clinician signature",

    // 7.Clinician Signatures - Designation
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician/practitioner_role/designation": "Designation 48",

      //BOILERPLATE
      "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician/name/use|code": "at0002",
    // 7.Clinician Signatures - Clinician Name
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician/name/text": "Dr Miller",
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician/identifier/value": "12345",

    //BOIlERPLATE
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician/identifier/value|issuer": "ProfessionalID",
      //BOIlERPLATE
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician/identifier/value|assigner": "ProfessionalID",

    //BOIlERPLATE
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician:0/identifier/value|type": "ProfessionalID",

    //BOIlERPLATE
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/signing_clinician:0/identifier/use|code": "at0004",


    // 7.Clinician Signatures - Date signed
    "nss_respect_form/respect_headings/a7._clinician_signatures/clinician_signature:0/time": "2019-03-03T23:09:53.12+01:00",


    // 8. Emergency contacts
    //BOILERPLATE
    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/name": "ReSPECT emergency contacts",

    // 8. Emergency contacts - role
    // Valueset:
    // Legal proxy or parent
    // Family or friend or other
    // GP
    // Lead consultant

    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/participant:0/role": "GP",
    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/participant:0/contact/name/use|code": "at0002",
    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/participant:0/contact/name:0/text": "Text 35",
    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/participant:0/contact/telephone/system|code": "at0012",
    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/participant:0/contact/telephone/telephone_number": "Telephone number 60",

    "nss_respect_form/respect_headings/a8._emergency_contacts/emergency_contacts/other_details": "Other details 70",

    // BOILERPLATE
    "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/ism_transition/current_state|value": "completed",
  // BOILERPLATE
    "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/service_name": "Respect form - confirmation of validity",
    "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/practitioner_role/designation": "Designation 26",
 "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/name/use|code": "at0002",

 "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/name/text": "Text 84",

 "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/identifier/value": "c3507fd3-a867-4dea-b9d7-9f1f0fc996d3",

   // BOILERPLATE
   "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/identifier/
value|issuer": "ProfessionalID",

  // BOILERPLATE
 "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/identifier/value|assigner": "ProfessionalID",

   // BOILERPLATE
 "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/identifier/value|type": "ProfessionalID",

  // BOILERPLATE
 "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/responsible_clinician/identifier/use|code": "at0004",

 // 9. Validity REview date
    "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/review_date": "2019-04-23",
// 9. Validity Date signed    
    "nss_respect_form/respect_headings/a9._confirmation_of_validity/service:0/time": "2019-03-03T23:09:53.121+01:00"
}
```

#### Notes:
