#### Heading

### Top 3 Issues

#### Version:

1.2.0 - 1 Apr 2019 - updated templateId
1.1.1- 28 Feb 2019 - Corrected AQL query

#### TemplateID:
`IDCR - Top3issues.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```sql

select     a/uid/value as uid,     a/composer/name as author,     a/context/start_time/value as date_created,     b/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.issue.v0,'Issue 1']/items[at0001]/value/value as Issue_1_Name,     b/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.issue.v0,'Issue 1']/items[at0002]/value/value as Issue_1_Detail,     b/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.issue.v0,'Issue 2']/items[at0001]/value/value as Issue_2_Name,     b/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.issue.v0,'Issue 2']/items[at0002]/value/value as Issue_2_Detail,     b/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.issue.v0,'Issue 3']/items[at0001]/value/value as Issue_3_Name,     b/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.issue.v0,'Issue 3']/items[at0002]/value/value as Issue_3_Detail from EHR e[ehr_id/value='{{ehrId}}'] contains COMPOSITION a[openEHR-EHR-COMPOSITION.encounter.v1] contains OBSERVATION b[openEHR-EHR-OBSERVATION.story.v1] where a/name/value='Top issues'

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
  "ctx/composer_name": "Hildi McNicoll",
  "ctx/health_care_facility|id": "999999-345",
  "ctx/health_care_facility|name": "Home",
  "ctx/id_namespace": "NHS-UK",
  "ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
  "ctx/language": "en",
  "ctx/territory": "GB",
  "ctx/time": "2017-05-24T00:11:02.518+02:00",
    "top_issues/story_history:0/issue_1/issue_1_name": "Anxiety",
    "top_issues/story_history:0/issue_1/issue_1_detail": "I feel very anxious all the time",
    "top_issues/story_history:0/issue_2/issue_2_name": "Not sleeping",
    "top_issues/story_history:0/issue_2/issue_2_detail": "I wake up in the middle of the night and can't get back to sleep",
    "top_issues/story_history:0/issue_3/issue_3_name": "No appetite",
    "top_issues/story_history:0/issue_3/issue_3_detail": "I don't like my food as much as I used to"
}
```

#### Notes:
None
