
    
    

    pipeline 
    {
        agent any
        environment {
            // Removed other variables for clarity...
            SFDX_USE_GENERIC_UNIX_KEYCHAIN = true
            def toolbelt = tool 'toolbelt'
            def JWT_KEY_CRED_ID = '06cfadcf-4f56-4ea7-80ea-692fc39f6456'
            def HUB_CLIENT_ID = '3MVG9fe4g9fhX0E7pjhub2D2EGJrfiStvD8NIFYxGr3tzXpCHoMV_aojlzCgAs4VBSqumxVYTkTmIb2mQy6d8'
            def HUB_USERNAME = 'davidvilla@sfdc.com'
            def HUB_HOST = 'https://login.salesforce.com'
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
  
