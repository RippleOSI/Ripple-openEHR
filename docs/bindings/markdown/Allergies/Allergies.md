#### Heading

### Allergies

#### Version:

1.0.0

18-Dec-2016

#### TemplateID:
`IDCR - Adverse Reaction List.v1`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
  a/uid/value as uid,
  a/composer/name as author,
  a/context/start_time/value as date_created,
  b_a/data[at0001]/items[at0002]/value/value as cause,
  b_a/data[at0001]/items[at0002]/value/defining_code/code_string as cause_code,
  b_a/data[at0001]/items[at0002]/value/defining_code/terminology_id/value as cause_terminology,
  b_a/data[at0001]/items[at0009]/items[at0011]/value/value as reaction,
  b_a/data[at0001]/items[at0009]/items[at0011]/value/defining_code/code_string as reaction_code,
  b_a/data[at0001]/items[at0009]/items[at0011]/value/terminology_id/value as reaction_terminology,
  b_a/provider/external_ref/id/value as originalComposition,
  b_a/provider/name as originalSource
 from EHR e [ehr_id/value = '{{ehrId}}']
  contains COMPOSITION a[openEHR-EHR-COMPOSITION.adverse_reaction_list.v1]
  contains EVALUATION b_a[openEHR-EHR-EVALUATION.adverse_reaction_risk.v1]
 where
  a/name/value='Adverse reaction list'

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
  "ctx/composer_name": "Dr Ian Shannon",
  "ctx/health_care_facility|id": "999999-345",
  "ctx/health_care_facility|name": "Home",
  "ctx/id_namespace": "NHS-UK",
  "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
  "ctx/language": "en",
  "ctx/territory": "GB",
  "ctx/time": "2016-12-20T00:11:02.518+02:00",
  "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/_provider|name": "iHEALTHLINK Discharge",
 "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/_provider|id": "http://showcase.1.rippleosi.org/1234567",
  "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/causative_agent|code": "91936005",
   "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/causative_agent|value": "allergy to penicillin",
   "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/causative_agent|terminology": "SNOMED-CT",
   "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/reaction_details/manifestation:0|code": "28926001",
   "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/reaction_details/manifestation:0|value": "eruption due to drug",
   "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/reaction_details/manifestation:0|terminology": "SNOMED-CT",
   "adverse_reaction_list/allergies_and_adverse_reactions/adverse_reaction_risk:0/reaction_details/comment": "History unclear"
}

```

#### Notes:

None
