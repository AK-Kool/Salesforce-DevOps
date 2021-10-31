node 
{
    
    def toolbelt = tool 'toolbelt'
    def JWT_KEY_CRED_ID = '7d1749fb-6d30-4f0c-9945-62fa99dba44f'
    def HUB_CLIENT_ID = '3MVG9fe4g9fhX0E7pjhub2D2EGJrfiStvD8NIFYxGr3tzXpCHoMV_aojlzCgAs4VBSqumxVYTkTmIb2mQy6d8'
    def HUB_USERNAME = 'davidvilla@sfdc.com'
    def HUB_HOST = 'https://login.salesforce.com'

    pipeline 
    {
        agent any
        environment {
            // Removed other variables for clarity...
            SFDX_USE_GENERIC_UNIX_KEYCHAIN = true
            // ...
        }
        stages {    
            stage('TEST') {
                steps {
                    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'VAR_CERT_FILE')]) {
                        sh returnStdout: true, script: "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${HUB_CLIENT_ID} --username ${HUB_USERNAME} --jwtkeyfile ${VAR_CERT_FILE} --setdefaultdevhubusername --instanceurl ${HUB_HOST}"
                    }
                }
            }
        }
    }
}    
