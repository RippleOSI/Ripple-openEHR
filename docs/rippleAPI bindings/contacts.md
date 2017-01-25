#### Heading

### Contacts

#### Version:

1.0.0

18-Dec-2016

#### TemplateID:
`IDCR - Relevant contacts.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/context/start_time/value as dateCreated,
    a/composer/name as author,
    b_b/data[at0001]/items[at0030]/value/value as relationshipRoleType,
    b_b/data[at0001]/items[at0025]/value/value as next_of_kin,
    b_b/data[at0001]/items[at0017]/value/value as notes,
    b_b/data[at0001]/items[at0035]/value/value as relationshipCategory,
    b_b/data[at0001]/items[at0035]/value/defining_code/code_string as relationshipCategoryCode,
    b_b/data[at0001]/items[at0035]/value/defining_code/terminologyId/value as relationshipCategoryTerminology,
    b_b/data[at0001]/items[openEHR-EHR-CLUSTER.individual_person_uk.v1]/items[openEHR-EHR-CLUSTER.person_name.v1]/items[at0001]/value/value as name,
    b_b/data[at0001]/items[openEHR-EHR-CLUSTER.individual_person_uk.v1]/items[openEHR-EHR-CLUSTER.address_uk.v0]/items[at0002]/value/value as address,
    b_b/data[at0001]/items[openEHR-EHR-CLUSTER.individual_person_uk.v1]/items[openEHR-EHR-CLUSTER.telecom_uk.v1]/items[at0002]/value/value as contactInformation
 from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.health_summary.v1]
contains (
    ADMIN_ENTRY b_b[openEHR-EHR-ADMIN_ENTRY.relevant_contact_rcp.v1])
where
    a/name/value='Relevant Contacts List'
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
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/individual_person/person_name/unstructured_name": "Janice Cox",
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/individual_person/address/address_description": "52 simmons St, Leeds",
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/individual_person/contact_details:0/comms_description": "Mobile: 045567 33452",
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/relationship_category|code": "at0037",
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/relationship_role": "Daughter",
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/is_next_of_kin": true,
  "relevant_contacts_list/relevant_contacts/relevant_contact:0/note": "Do not call overnight."
}

```

#### Notes:

None
