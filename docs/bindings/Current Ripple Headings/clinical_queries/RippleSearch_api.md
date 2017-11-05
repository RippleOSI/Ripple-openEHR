# RippleSearch 'View' Endpoints

Think!EHR exposes a custom /view resource which perform a population clinical query and return a count of patients found within a set of Age Bands.

## /view AgeBandSearch

```
GET /rest/v1/view/4ee4bad9-2f9e-4e33-b1d6-6572709cabee/AgeBandsSearch?textValue=hip&amp;yearofBirth=&amp;snomedCode&amp;headingType=Procedures&amp;minAge=&amp;maxAge=&amp;genderCode=at0009 HTTP/1.1
Host: ehrscape.code4health.org
Authorization: Basic YzRoX3JpcHBsZV9vc2k6SW5EZUNvTVA=
```

Notes:

The `4ee4bad9-2f9e-4e33-b1d6-6572709cabee/` aspect of the URL is an ehr_id but is hardwired and does not need to be changed

### Parameters

`headingType`:

* **Mandatory**.
* One of `Allergies`,`Medications`,`Problems`, `Procedures`

`snomedCode`:

* A SNOMED CT code such as `14567832`. Exact matching is performed.

`textValue`:

* A text value such as `Salbutamol`. Case-insensitive, partial matching is performed.

`gender`:

* `M` for Male only : `at0009`
* `F` for Female only: `at0010`
* omit for both

`yearofBirth`:

* A year of birth as string e.g. `1944`

`minAge`:

* The minimum age of the search e.g. `25`

`maxAge`:

* The maximum age of the search e.g. `72`


### Returns:

 A Json object carrying counts for each Age band and a total, such as

```
{
  "agedElevenToEighteen": 0,
  "agedNineteenToThirty": 3,
  "agedThirtyOneToSixty": 0,
  "agedSixtyOneToEighty": 22,
  "agedEightyPlus": 12,
  "all": 37,
  "resultset":
  {
    ehrId: "123456-3456-45345",
    subjectId: "9999999000",
    yearofBirth: "1946"
  }
}
```


## Think!EHR View Code ( internal use only)
AgeBandsSearch

## Description
Return counts of clinical queries Age bands

## Meta data
```Json
{
    "parameters": [
        {
            "name": "snomedCode",
            "type": "string",
            "format": "string",
            "description": "The SNOMED code to be found."
        },
        {
            "name": "queryText",
            "type": "string",
            "format": "string",
            "description": "The text value to be found (case-sensitive) and partial match allowed."
        },
        {
            "name": "headingType",
            "type": "string",
            "format": "string",
            "description": "Mandatory. The target header to search for - Allergies,Medications,Problems, Procedures."
        },
        {
            "name": "gender",
            "type": "string",
            "format": "string",
            "description": "M for males only F for females, omit for both"
        },
        {
            "name": "yearofBirth",
            "type": "string",
            "format": "string",
            "description": "The target Year of Birth for which to search."
        },
        {
            "name": "minAge",
            "type": "integer",
            "format": "integer",
            "description": "The minimum age for which to search (optional)."
        },
        {
            "name": "maxAge",
            "type": "integer",
            "format": "integer",
            "description": "The maximum age for which to search (optional)."
        }                   
    ]
}
```

## Data
```javascript
function formatCodeTextSearchClause(ctx, namePath) {
 var aql = "";

  if (ctx.vars.snomedCode) {
      aql = aql + ' and '+ namePath + '/defining_code/code_string matches {\'' + ctx.vars.snomedCode + '\'} and ' + namePath + '/defining_code/terminology_id/value = \'SNOMED-CT\''
  }

  if (ctx.vars.queryText) {
    aql =  aql + ' and '+ namePath + '/value like \'*' + ctx.vars.queryText + '*\''
  }

 return aql;

}
function getAllergyAQL(ctx) {
   var aql =  ' contains EVALUATION a_a[openEHR-EHR-EVALUATION.adverse_reaction_risk.v1]' +
         ' where a/name/value matches {\'Adverse reaction list\'}' +
          this.formatCodeTextSearchClause(ctx, 'a_a/data[at0001]/items[at0002]/value');
    return aql;
};

function getProcedureAQL(ctx) {
   var aql =  ' contains ACTION a_a[openEHR-EHR-ACTION.procedure.v1]' +
         ' where a/name/value matches {\'Procedures list\'}' +
          this.formatCodeTextSearchClause(ctx, 'a_a/description[at0001]/items[at0002]/value');
    return aql;
};

function getProblemAQL(ctx) {
   var aql = ' contains EVALUATION a_a[openEHR-EHR-EVALUATION.problem_diagnosis.v1]' +
         ' where a/name/value matches {\'Problem list\'}' +
        this.formatCodeTextSearchClause(ctx, 'a_a/data[at0001]/items[at0002]/value');
      return aql;
};

function getMedicationAQL(ctx) {

      var aql = ' contains INSTRUCTION a_a[openEHR-EHR-INSTRUCTION.medication_order.v1]' +
           ' where a/name/value matches {\'Medication list\'}' +
           this.formatCodeTextSearchClause(ctx, 'a_a/activities[at0001]/description[at0002]/items[at0070]/value');
      return aql;
    };


function formatQuery(ctx, minAgeBand,maxAgeBand,countOnly) {

            var now = new Date();
            var currentYear = now.getFullYear();

            var minBandYear = currentYear - maxAgeBand;
            var maxBandYear = currentYear - minAgeBand;
            var aql;

            if (countOnly) {
              aql = 'select count(e) as cnt from EHR e contains COMPOSITION a'
            }
            else {
              aql = 'select distinct e/ehr_id/value as ehrId, e/ehr_status/subject/external_ref/id/value as subjectId, e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value as yearofBirth from EHR e contains COMPOSITION a '           
            };

            if  (ctx.vars.headingType === 'Allergies')
              {
              aql = aql + this.getAllergyAQL(ctx)
              }
            else if (ctx.vars.headingType === 'Problems')
              {
               aql = aql + this.getProblemAQL(ctx)
              }
              else if (ctx.vars.headingType === 'Procedures')
                {
                 aql = aql + this.getProcedureAQL(ctx)
                }
            else if (ctx.vars.headingType === 'Medications')
              {
               aql = aql + this.getMedicationAQL(ctx)
              };


            aql = aql + ' and e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value >= \'' + minBandYear + '\'' ;
            aql = aql + ' and e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value <= \'' + maxBandYear + '\'';

          if (ctx.vars.yearofBirth) {
               aql = aql +  ' and e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value = \'' + ctx.vars.yearofBirth + '\'';
          }

          if (ctx.vars.minAge) {
            aql = aql + ' and e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value >= \'' + (currentYear - ctx.vars.maxAge).toString() + '\'';
          }

         if (ctx.vars.maxAge) {
            aql = aql + ' and e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value <= \'' + (currentYear - ctx.vars.minAge).toString() + '\'';
          }

         if (ctx.vars.gender) {
            var genderCode;
            if (ctx.vars.gender = 'M')
            {
              genderCode = 'at0009';
            }
            else
            if (ctx.vars.gender = 'F') {
              genderCode = 'at0010';        
            }

            aql = aql +' and e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0001]/value/value >= \'' + genderCode + '\'';
          }

           aql = aql +' ORDER BY e/ehr_status/other_details/items[openEHR-EHR-CLUSTER.person_anonymised_parent.v1]/items[at0014]/value/value';

       return aql;
      }

function compute(ctx, src) {

    var promises = {

        agedElevenToEighteen: Ehr.query({
            aql: formatQuery(ctx,11,18,true),
            params: ctx.vars,
            callback: function (out, cnt2) {
                out.count = cnt2.cnt;
            }
        }),
        agedNineteenToThirty: Ehr.query({
            aql: formatQuery(ctx,19,30,true),
            params: ctx.vars,
            callback: function (out, cnt2) {
                out.count = cnt2.cnt;
            }
        }),
        agedThirtyOneToSixty: Ehr.query({
            aql: formatQuery(ctx,31,60,true),
            params: ctx.vars,
            callback: function (out, cnt2) {
                out.count = cnt2.cnt;
            }
        }),
        agedSixtyOneToEighty: Ehr.query({
            aql: formatQuery(ctx,61,80,true),
            params: ctx.vars,
            callback: function (out, cnt2) {
                out.count = cnt2.cnt;
            }
        }),
        agedEightyPlus: Ehr.query({
            aql: formatQuery(ctx,81,250,true),
            params: ctx.vars,
            callback: function (out, cnt2) {
                out.count = cnt2.cnt;
            }
        }),
        all: Ehr.query({
            aql: formatQuery(ctx,0,250,true),
            params: ctx.vars,
            callback: function (out, cnt2) {
                out.count = cnt2.cnt;
            }
        }),
        resultset: Ehr.query({
            aql: formatQuery(ctx,0,250,false),
            params: ctx.vars,
            initvalue: [],
            callback: function (out, qry) {
                 out.push({
                    "ehrId": qry.ehrId,
                    "subjectId": qry.subjectId ,
                    "yearofBirth": qry.yearofBirth
                 });
            }
        })
    };

    var result = {};
    Ehr.allhash(promises, function (res) {
        result = {
            agedElevenToEighteen: res['agedElevenToEighteen'].count,
            agedNineteenToThirty: res['agedNineteenToThirty'].count,
            agedThirtyOneToSixty: res['agedThirtyOneToSixty'].count,
            agedSixtyOneToEighty: res['agedSixtyOneToEighty'].count,
            agedEightyPlus: res['agedEightyPlus'].count,
            all: res['all'].count,
            resultset: res['resultset']
        };
    });

    return result;
}
```
