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
[Summary AQL](./summary_detail.aql)
```

#### Detail AQL /query:
To populate the detailed view / edit when a single record within the heading is selected.
Identical to summary AQL.

```
[Summary AQL](./summary_detail.aql)
```

#### Sample Composition(FLAT JSON) for POST/PUT /composition:

To create or update a composition for a single item via the /composition Ehrscape API call.

```
[Example FLAT JSON composition](./detail_FLAT.json)

```

#### Notes:

None

### FHIR Care-Connect Mappings

Target: [FHIR CareConnect-AllergyIntolerance-1 profile]
(http://www.interopen.org/candidate-profiles/care-connect/CareConnect-AllergyIntolerance-1.html)

Conformance

create
read
search
conformance


Supported Resource search criteria

https://www.hl7.org/fhir/allergyintolerance.html#search

date
identifier
manifestation
patient
recorder
status (always active in Ripple)
substance


read

RippleFHIR
