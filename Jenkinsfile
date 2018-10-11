#!/usr/bin/env groovy

import groovy.json.JsonOutput
node {
        notify1('Started testing-whaleapp build')
        try {
			stage('Checkout') {
			checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/chilu49/hellowhale.git']]])
			}
		
		stage('cleanup') {
				sh ''' docker stop testing-whaleapp'''
				sh ''' docker rm testing-whaleapp '''
				sh ''' docker images --no-trunc --format '{{.ID}} {{.CreatedSince}} {{.Repository}}' |grep whaleapp|awk '{print $1}'|xargs --no-run-if-empty docker rmi -f '''
				}
 			stage('Build Image And Push') {
			dir('/opt/jenkins/workspace/testing-asdfkljasdf/'){
						
 						def app = docker.build ("testing-whaleapp:${env.BUILD_ID}")
						}
					}
			
    			stage ('Deploy-dev') {
				//build job: 'account-service-pipeline', wait: false
				//sh ''' docker stop testing-whaleapp'''
				//sh ''' docker rm testing-whaleapp '''
				sh ''' docker run -d  -it -p 8888:80 --name testing-whaleapp  testing-whaleapp:${BUILD_NUMBER}'''
				sh '''sleep 10'''
				   
       }    
        notify2('Successfully Deployed testing-whaleapp')  
	notifySlack(currentBuild.result)
	
    }  catch (err) {
        notify1("Error {err}")
        currentBuild.result = 'FAILURE'
	notifySlack(currentBuild.result)
    }
}   
@NonCPS
def getChangeLog(passedBuilds) {
    def changeString = ""
    for (int x = 0; x < passedBuilds.size(); x++) {
        def currentBuild = passedBuilds[x];
        def changeLogSets = currentBuild.changeSets
        for (int i = 0; i < changeLogSets.size(); i++) {
            def entries = changeLogSets[i].items
            for (int j = 0; j < entries.length; j++) {
                def entry = entries[j]
                changeString += "* ${entry.msg} by ${entry.author} \n"
            }
        }
    }
if (!changeString) {
changeString = " - No new changes"
    }
    return log;
}

@NonCPS
def getChangeString() {
    MAX_MSG_LEN = 1000
    def changeString = ""
    echo "Gathering SCM changes"
    def changeLogSets = currentBuild.changeSets
	echo "${changeLogSets}"
	echo "${changeLogSets.size()}" 
    for (int i = 0; i < changeLogSets.size(); i++) {
    	def entries = changeLogSets[i].items
    	for (int j = 0; j < entries.length; j++) {
        	def entry = entries[j]
            	truncated_msg = entry.msg.take(MAX_MSG_LEN)
 		changeString += " - ${truncated_msg} [${entry.author}]\n"
        }
    }

    if (!changeString) {
        changeString = " - No new changes"
    }
    return changeString
    }
    
	def notify1(status){
	emailext(
		to: "ravinder.chiluveru@cabelas.com",
		subject: "${status}:' [${env.BUILD_NUMBER}]'",
		body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
			<p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME}  [${env.BUILD_NUMBER}]</a></p>"""
			)
    }
	def notify2(status){
	emailext(
		to: "ravinder.chiluveru@cabelas.com",
		subject: "${status}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
		body: "Changes:\n " + getChangeString() + "\n\n<br> Check console output at: $BUILD_URL/console" + "\n"
			)
    }
	def notify3(status){
	emailext(
		to: "ravinder.chiluveru@cabelas.com",
		subject: "${status}:' [${env.BUILD_NUMBER}]'",
		body: "Changes:\n " + getChangeString() + "\n\n<br> Check console output at: $BUILD_URL/console" + "\n"
			)
    } 
	
	def notifySlack(String buildStatus = 'STARTED') {
    		// Build status of null means success.
    		buildStatus = buildStatus ?: 'SUCCESS'

    		def color

    		if (buildStatus == 'STARTED') {
        		color = '#D4DADF'
    		} else if (buildStatus == 'SUCCESS') {
        	color = '#BDFFC3'
    		} else if (buildStatus == 'UNSTABLE') {
        	color = '#FFFE89'
    		} else {
        	color = '#FF9FA1'
    		}

    		def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL} \n Please click on either proceed to deploy in test or abort to stop the build"

    		//slackSend(color: color, message: msg)
    		slackSend baseUrl: 'https://cabelasmobility.slack.com/services/hooks/jenkins-ci/', channel: 'newcoebb-buildstatus', color: 'color', message: msg, tokenCredentialId: 'jenkins-slack-integration-new'
		}


		
	
