#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG="davidvilla@sfdc.com"
    def SFDC_HOST = "https://login.salesforce.com"
    def JWT_KEY_CRED_ID = '06cfadcf-4f56-4ea7-80ea-692fc39f6456'
    def CONNECTED_APP_CONSUMER_KEY="3MVG9fe4g9fhX0E7pjhub2D2EGJrfiStvD8NIFYxGr3tzXpCHoMV_aojlzCgAs4VBSqumxVYTkTmIb2mQy6d8"
    def toolbelt = tool 'toolbelt'
	
    println 'KEY IS' 
    	
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:jwt:grant --clientid 3MVG9fe4g9fhX0E7pjhub2D2EGJrfiStvD8NIFYxGr3tzXpCHoMV_aojlzCgAs4VBSqumxVYTkTmIb2mQy6d8 --jwtkeyfile ${jwt_key_file} --username davidvilla@sfdc.com --instanceurl https://login.salesforce.com --setdefaultdevhubusername"
            }else{
		    //bat "${toolbelt} plugins:install salesforcedx@49.5.0"
		    bat "${toolbelt} update"
		    //bat "${toolbelt} auth:logout -u ${HUB_ORG} -p" 
                 rc = bat returnStatus: true, script: "${toolbelt} auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --loglevel DEBUG --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            }
		
            if (rc != 0) { 
		    println 'inside rc 0'
		    error 'hub org authorization failed' 
	    }
		else{
			println 'rc not 0'
		}

			println rc
			
			// need to pull out assigned username
			if (isUnix()) {
				//rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
				rmsg = sh returnStdout: true, script: "${toolbelt}/sfdx force:source:deploy -x manifest/package.xml -u ${HUB_ORG}"
			}else{
				rmsg = bat returnStdout: true, script: "${toolbelt} force:source:deploy -x manifest/package.xml -u ${HUB_ORG}"
			   //rmsg = bat returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			}
			  
            printf rmsg
            println('Hello from a Job DSL script!')
            println(rmsg)
        }
    }
}
