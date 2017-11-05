#### Heading

### Vital Signs

#### Version:

1.0.0

18-Dec-2016

#### TemplateID:
`IDCR - Vital Signs Encounter.v1`

#### Summary AQL /query:

To populate the list of items when the heading is selected.

```
select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as dateCreated,
    b_a/items[at0057]/value/value as onAir,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude as respiratoryRate,
    b_c/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value/magnitude as heartRate,
    b_d/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value/magnitude as temperature,
    b_f/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/defining_code/code_string as levelOfConsciousnessCode,
    b_g/data[at0001]/events[at0006]/data[at0003]/items[at0004]/value/magnitude as systolic,
    b_g/data[at0001]/events[at0006]/data[at0003]/items[at0005]/value/magnitude as diastolic,
    b_h/data[at0001]/events[at0002]/data[at0003]/items[at0006]/value/numerator as oxygenSats,
    b_i/data[at0001]/events[at0002]/data[at0003]/items[at0028]/value/magnitude as newsScore
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a[openEHR-EHR-COMPOSITION.encounter.v1]
contains (
    CLUSTER b_a[openEHR-EHR-CLUSTER.ambient_oxygen.v0] or
    OBSERVATION b_b[openEHR-EHR-OBSERVATION.respiration.v1] or
    OBSERVATION b_c[openEHR-EHR-OBSERVATION.pulse.v1] or
    OBSERVATION b_d[openEHR-EHR-OBSERVATION.body_temperature.v1] or
    OBSERVATION b_f[openEHR-EHR-OBSERVATION.avpu.v1] or
    OBSERVATION b_g[openEHR-EHR-OBSERVATION.blood_pressure.v1] or
    OBSERVATION b_h[openEHR-EHR-OBSERVATION.indirect_oximetry.v1] or
    OBSERVATION b_i[openEHR-EHR-OBSERVATION.news_uk_rcp.v1])
where a/name/value='Vital Signs Observations'
```

#### Detail AQL /query:
To populate the detailed view / edit when a single record within the heading is selected.

```
As above
```

### Time-series query
This returns *any* Vital signs records made, irrespective of the original composition i.e not just the formal vital signs compositions. This may be helpful for constructing a graph.

select
    a/uid/value as uid,
    a/composer/name as author,
    a/context/start_time/value as dateCreated,
    b_a/items[at0057]/value/value as onAir,
    b_b/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude as respiratoryRate,
    b_c/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value/magnitude as heartRate,
    b_d/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value/magnitude as temperature,
    b_f/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/defining_code/code_string as levelOfConsciousnessCode,
    b_g/data[at0001]/events[at0006]/data[at0003]/items[at0004]/value/magnitude as systolic,
    b_g/data[at0001]/events[at0006]/data[at0003]/items[at0005]/value/magnitude as diastolic,
    b_h/data[at0001]/events[at0002]/data[at0003]/items[at0006]/value/numerator as oxygenSats,
    b_i/data[at0001]/events[at0002]/data[at0003]/items[at0028]/value/magnitude as newsScore
from EHR e [ehr_id/value = '{{ehrId}}']
contains COMPOSITION a
contains (
    CLUSTER b_a[openEHR-EHR-CLUSTER.ambient_oxygen.v0] or
    OBSERVATION b_b[openEHR-EHR-OBSERVATION.respiration.v1] or
    OBSERVATION b_c[openEHR-EHR-OBSERVATION.pulse.v1] or
    OBSERVATION b_d[openEHR-EHR-OBSERVATION.body_temperature.v1] or
    OBSERVATION b_f[openEHR-EHR-OBSERVATION.avpu.v1] or
    OBSERVATION b_g[openEHR-EHR-OBSERVATION.blood_pressure.v1] or
    OBSERVATION b_h[openEHR-EHR-OBSERVATION.indirect_oximetry.v1] or
    OBSERVATION b_i[openEHR-EHR-OBSERVATION.news_uk_rcp.v1])
```


#### Sample Composition(FLAT JSON) for POST/PUT /composition:

To create or update a composition for a single item via the /composition Ehrscape API call.

```
{
"ctx/composer_name": "Dr Tony Shannon",
"ctx/composer_id": "G33567",
"ctx/health_care_facility|id": "999999-345",
"ctx/health_care_facility|name": "Ripple Valley Hospital NHS",
"ctx/id_namespace": "NHS-UK",
"ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",
"ctx/language": "en",
"ctx/territory": "GB",
"ctx/time": "2015-08-03T00:23:02.518+02:00",
    "vital_signs_observations/vital_signs/respirations/rate|magnitude": 23,
    "vital_signs_observations/vital_signs/respirations/rate|unit": "/min",
    "vital_signs_observations/vital_signs/respirations/inspired_oxygen/on_air": false,
    "vital_signs_observations/vital_signs/pulse_heart_beat/heart_rate|magnitude": 45,
    "vital_signs_observations/vital_signs/pulse_heart_beat/heart_rate|unit": "/min",
    "vital_signs_observations/vital_signs/body_temperature/temperature|magnitude": 35.4,
    "vital_signs_observations/vital_signs/body_temperature/temperature|unit": "°C",
    "vital_signs_observations/vital_signs/avpu/avpu_observation|code": "at0005",
    "vital_signs_observations/vital_signs/blood_pressure/systolic|magnitude": 92,
    "vital_signs_observations/vital_signs/blood_pressure/systolic|unit": "mm[Hg]",
    "vital_signs_observations/vital_signs/blood_pressure/diastolic|magnitude": 64,
    "vital_signs_observations/vital_signs/blood_pressure/diastolic|unit": "mm[Hg]",
    "vital_signs_observations/vital_signs/indirect_oximetry/spo2|numerator": 97,
    "vital_signs_observations/vital_signs/indirect_oximetry/spo2|denominator": 103,
    "vital_signs_observations/vital_signs/news_uk_rcp/total_score": 12
}
```

*Annotated JSON*
```
{
//author
"ctx/composer_name": "Dr Tony Shannon",
"ctx/composer_id": "G33567",

//Health care facility (currently hsardwired)
"ctx/health_care_facility|id": "999999-345",
"ctx/health_care_facility|name": "Ripple Valley Hospital NHS",

//Identifier namescaping (currently hardwired)
"ctx/id_namespace": "NHS-UK",
"ctx/id_scheme": "2.16.840.1.113883.2.1.4.3",

//Hardwired language and territory
"ctx/language": "en",
"ctx/territory": "GB",

// Date created/recorded
"ctx/time": "2015-08-03T00:23:02.518+02:00",

//Respiration rate
"vital_signs_observations/vital_signs/respirations/rate|magnitude": 23,
"vital_signs_observations/vital_signs/respirations/rate|unit": "/min",

//Supplemental Oxygen
//Note value is reverse of UI
// Supplemental Oxygen = true => on_air = false
// Supplemental Oxygen = false => on_air = true
"vital_signs_observations/vital_signs/respirations/inspired_oxygen/on_air": false,

//Heart rate
"vital_signs_observations/vital_signs/pulse_heart_beat/heart_rate|magnitude": 45,
"vital_signs_observations/vital_signs/pulse_heart_beat/heart_rate|unit": "/min",

//Temperature
"vital_signs_observations/vital_signs/body_temperature/temperature|magnitude": 35.4,
"vital_signs_observations/vital_signs/body_temperature/temperature|unit": "°C",

// Level of consciousness
"vital_signs_observations/vital_signs/avpu/avpu_observation|code": "at0005",
"vital_signs_observations/vital_signs/blood_pressure/systolic|magnitude": 92,

//Systolic
"vital_signs_observations/vital_signs/blood_pressure/systolic|unit": "mm[Hg]",
"vital_signs_observations/vital_signs/blood_pressure/diastolic|magnitude": 64,
//Diastolic
"vital_signs_observations/vital_signs/blood_pressure/diastolic|unit": "mm[Hg]",

// Oxygen saturation
//Denominator always 100    
"vital_signs_observations/vital_signs/indirect_oximetry/spo2|numerator": 97,
"vital_signs_observations/vital_signs/indirect_oximetry/spo2|denominator": 103,

//NEWS Score
"vital_signs_observations/vital_signs/news_uk_rcp/total_score": 12
}
```
#### Notes:
None. Tested and working on
