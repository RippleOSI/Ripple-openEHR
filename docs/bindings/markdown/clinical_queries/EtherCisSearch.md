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
   var aql =  'contains COMPOSITION a[openEHR-EHR-COMPOSITION.adverse_reaction_list.v1] contains EVALUATION a_a[openEHR-EHR-EVALUATION.adverse_reaction_risk.v1]' +
         ' where a/name/value matches {\'Adverse reaction list\'}' +
          this.formatCodeTextSearchClause(ctx, 'a_a/data[at0001]/items[at0002]/value');
    return aql;
};

function getProcedureAQL(ctx) {
   var aql =  'contains COMPOSITION a[openEHR-EHR-COMPOSITION.health_summary.v1] contains ACTION a_a[openEHR-EHR-ACTION.procedure.v1]' +
         ' where a/name/value matches {\'Procedures list\'}' +
          this.formatCodeTextSearchClause(ctx, 'a_a/description[at0001]/items[at0002]/value');
    return aql;
};

function getProblemAQL(ctx) {
   var aql = 'contains COMPOSITION a[openEHR-EHR-COMPOSITION.problem_list.v1] contains EVALUATION a_a[openEHR-EHR-EVALUATION.problem_diagnosis.v1]' +
         ' where a/name/value matches {\'Problem list\'}' +
        this.formatCodeTextSearchClause(ctx, 'a_a/data[at0001]/items[at0002]/value');
      return aql;
};

function getMedicationAQL(ctx) {

      var aql = ' contains COMPOSITION a[openEHR-EHR-COMPOSITION.medication_list.v0] contains INSTRUCTION a_a[openEHR-EHR-INSTRUCTION.medication_order.v1]' +
           ' where a/name/value matches {\'Medication list\'}' +
           this.formatCodeTextSearchClause(ctx, 'a_a/activities[at0001]/description[at0002]/items[at0070]/value');
      return aql;
    };


function formatQuery(ctx) {

            var aql;

            aql = 'select distinct e/ehr_id/value as ehrId, e/ehr_status/subject/external_ref/id/value as subjectId, from EHR e '           

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

       return aql;
      }
