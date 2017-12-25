trigger SetContactPrediction on Contact (after insert, after update) {
    if(system.isFuture()) return;
    
    // Name of the custom setting
    private static final String CONFIG_NAME = 'Einstein';
    
    // get config
    analytics_edd__SDDPredictionConfig__c config =
        analytics_edd__SDDPredictionConfig__c.getInstance(CONFIG_NAME);
    
    List<Map<String, String>> fieldsList = new List<Map<String, String>>();
    
    for (sObject sObj: Trigger.new) {
        Map<String, Schema.SObjectField> objectFields =
            Schema.getGlobalDescribe().get(config.analytics_edd__object_api_name__c).getDescribe().fields.getMap();
        Map<String, String> fieldValues = new Map<String, String>();
        
        for (Schema.SObjectField field: objectFields.values()) {
            if (!field.getDescribe().getType().name().endsWithIgnoreCase('textarea')) {
                String fieldApiName = field.getDescribe().getName();
                fieldValues.put(fieldApiName, String.valueOf(sObj.get(fieldApiName)));
            }
        }
        
        // log field values
        System.debug(fieldValues);
        fieldsList.add(fieldValues);
    }
    
    // make setPrediction call
    analytics_edd.PredictionIntegration.setPrediction(CONFIG_NAME, JSON.serialize(fieldsList));
}
