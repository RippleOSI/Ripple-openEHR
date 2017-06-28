#### Heading

### Events

#### Version:
1.0.0
1.0.1
added event Time to AQL and composition sample

05-Jun-2017

#### TemplateID:
`IDCR - Service tracker.v0`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as date_created,
    b_a/description[at0001]/items[at0011]/value/value as eventName,
    b_a/description[at0001]/items[at0014]/value/value as eventType,
    b_a/description[at0001]/items[at0013]/value/value as eventDescription,
    b_a/time as eventTime
from EHR e
contains COMPOSITION a[openEHR-EHR-COMPOSITION.service_tracker.v0]
contains ACTION b_a[openEHR-EHR-ACTION.service.v0]
where a/name/value='Service tracker'
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
    "ctx/language": "en",
    "ctx/territory": "GB",
    "ctx/composer_name": "Dr Rory Best",
    "ctx/time": "2017-06-05T10:50:20.958Z",
    "ctx/id_namespace": "Ripple",
    "ctx/id_scheme": "Ripple",
    "ctx/health_care_facility|name": "Ripple General",
    "ctx/health_care_facility|id": "9091",
    "service_tracker/service:0/ism_transition/current_state|code": "526",
    "service_tracker/service:0/ism_transition/current_state|value": "planned",
    "service_tracker/service:0/service_name": "Event_name",
    "service_tracker/service:0/service_type": "Event_type",
    "service_tracker/service:0/description": "Notes",
    "service_tracker/service:0/time":"2017-06-05T10:50:20.958Z"
}

```

#### Notes:
```json
{
    //Boilerplate
    "ctx/language": "en",
    //Boilerplate
    "ctx/territory": "GB",
    //author name
    "ctx/composer_name": "Dr Rory Best",
    //document commit time
    "ctx/time": "2017-06-05T10:50:20.958Z",
    //Boilerplate
    "ctx/id_namespace": "Ripple",
    //Boilerplate
    "ctx/id_scheme": "Ripple",
    //Boilerplate
    "ctx/health_care_facility|name": "Ripple General",
    //Boilerplate
    "ctx/health_care_facility|id": "9091",
    //Boilerplate
    "service_tracker/service:0/ism_transition/current_state|code": "526",
    //Boilerplate
    "service_tracker/service:0/ism_transition/current_state|value": "planned",
    //Event name
    "service_tracker/service:0/service_name": "Event_name",
    //Event type
    "service_tracker/service:0/service_type": "Event_type",
    //Notes
    "service_tracker/service:0/description": "Notes",
    //Event time
    "service_tracker/service:0/time":"2017-06-05T10:50:20.958Z"
}
```
