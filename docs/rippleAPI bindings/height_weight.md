#### Heading

### Height and Weight

#### Version:

1.0.0

18-Dec-2016

#### TemplateID:
`RIPPLE - Height_Weight.v1`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
  b_a/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude as height,
  b_a/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/units as height_units,
  b_b/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value/magnitude as weight,
  b_b/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value/units as weight_units
from EHR e[ehr_id/value='{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.encounter.v1]
contains (
    OBSERVATION b_a[openEHR-EHR-OBSERVATION.height.v1]
    or
    OBSERVATION b_b[openEHR-EHR-OBSERVATION.body_weight.v1])
where a/name/value='Height Weight'

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
"ctx/time": "2016-01-24T00:11:02.518+02:00",
"height_weight/height_length/any_event:0/height|magnitude": 184,
"height_weight/height_length/any_event:0/height|unit": "cm",
"height_weight/body_weight/weight|magnitude": 72.5,
"height_weight/body_weight/weight|unit": "kg"
}
```

#### Notes:

None
