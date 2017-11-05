#### Heading

### Vital Signs

#### Version:

1.0.0

27-Jan-2017

#### TemplateID:
`IDCR - Vital Signs Encounter.v1`


To populate the list of items when the heading is selected.


[Summary AQL](./summary_detail.aql)


#### Detail AQL /query:
To populate the detailed view / edit when a single record within the heading is selected.

**Identical to summary AQL.**


[Summary AQL](./summary_detail.aql)


#### Sample Composition(FLAT JSON) for POST/PUT /composition:

To create or update a composition for a single item via the /composition Ehrscape API call.

[Example FLAT JSON composition with markup](./detail_FLAT.json)

[Example FLAT JSON composition](./detail_FLAT_sample.json)


#### Notes:

Note that the `onAir` datapoint in the openEHR models is the **logical opposite** of the `onSupplementalOxygen` Ripple UI field.
