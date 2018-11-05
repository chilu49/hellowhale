#!/usr/bin/env groovy

import groovy.json.JsonOutput
node {
	currentbuildnumber = "${BUILD_NUMBER}"
        notify1('Started testing-whaleapp build')
        try {
			stage('Checkout') {
			checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/chilu49/hellowhale.git']]])
			}
		
		stage('cleanup') {
				
				sh ''' docker stop testing-whaleapp && docker rm $_ '''
				sh ''' docker images --no-trunc --format '{{.ID}} {{.CreatedSince}} {{.Repository}}' |grep whaleapp|awk '{print $1}'|xargs --no-run-if-empty docker rmi -f '''
				}
 			stage('Build Image And Push') {
			dir('/opt/jenkins/workspace/testing-asdfkljasdf/'){
						
 						//def app = docker.build ("testing-whaleapp:${env.BUILD_ID}")
						docker.withRegistry('https://shqhbrap65.cabelas.corp/harbor/projects/3/repositories', 'harbour-docker-registry-cred'){
						def app = docker.build ("hellowhale-testing/hellowhale:${env.BUILD_ID}")
						app.push()
								}

						}
					}
			
    			stage ('Deploy-dev') {
				//build job: 'account-service-pipeline', wait: false
				//sh ''' docker stop testing-whaleapp'''
				//sh ''' docker rm testing-whaleapp '''
				sh ''' docker run -d  -it -p 8888:80 --name testing-whaleapp  hellowhale-testing/hellowhale:${BUILD_NUMBER}'''
				sh '''sleep 10'''
				
				sh "sed 's/BUILDNUMBER/currentbuildnumber/g' whaleapp-deployment.yaml
				//kubernetesDeploy configs: 'whaleapp-deployment.yaml', kubeConfig: [path: 'k8s-jenkins-dev-serviceaccount-development-conf'], kubeconfigId: 'jenkins-dev-serviceaccount', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURKRENDQWd5Z0F3SUJBZ0lVVm9WblRrOStiNDlCd1lnY2h4ZmJlTGNDdWQ0d0RRWUpLb1pJaHZjTkFRRUwKQlFBd0RURUxNQWtHQTFVRUF4TUNZMkV3SGhjTk1UZ3dOakk0TVRjMU1qQTFXaGNOTVRrd05qSTRNVGMxTWpBMQpXakFOTVFzd0NRWURWUVFERXdKallUQ0NBU0l3RFFZSktvWklodmNOQVFFQkJRQURnZ0VQQURDQ0FRb0NnZ0VCCkFLZHcyWU4xaGM2bTFXbHJuSnh3QzRxRWc0Wks4SHV6NWc4OUxqQTBTc0x2akhkeFdIOEsrNUlWS2lyTmtzancKUTkzd0dKV2gxUkhHdWtLQzkzVkQ0TFpIWHVOZEJ2eHZxam51S2pYS2h5RWJLVTdMcng5QzFzWmNGeTdRNzhrWgp0VUxWR3llTEoyY2wyZ2F3bGRGenZodVFSUkVKNkVxY25jbjVGY29sczhvTjZMYms4VFNrTzAwVzdoQlUzR0srCm0vU3BSUFhYVDBkVTFLRVJsMkQ3d1lnNU4yUTBvOXowSmVLZ0xpWWppUWRhdFM0M1RTdW1DQVpWaTB5Z0U1cFQKQTA2bXRnR0FsVlQyOWlxK3hqVk95YWFURWVoc2dUS3lSR1dteHpyWFdZUDUzWG80b0pxM2Nad0o3ZkZubUg1NQpuNGUrYUs2Y05pVWFOSnlrN0N4MGd6VUNBd0VBQWFOOE1Ib3dIUVlEVlIwT0JCWUVGSVQrOThOT01RQ216blFECjkyOWlxMzZ4bVdydk1FZ0dBMVVkSXdSQk1EK0FGSVQrOThOT01RQ216blFEOTI5aXEzNnhtV3J2b1JHa0R6QU4KTVFzd0NRWURWUVFERXdKallZSVVWb1ZuVGs5K2I0OUJ3WWdjaHhmYmVMY0N1ZDR3RHdZRFZSMFRBUUgvQkFVdwpBd0VCL3pBTkJna3Foa2lHOXcwQkFRc0ZBQU9DQVFFQUJZZTN0SXFNeVA1NjdzT3R3N0ZTT0Ira1FhSjZIV3dSClowVFowQTJKQ0t0eVRpOTZIelZqWFo0d0RGZWNiWXlvNkFsZXpEdzMxZmJVOEtSNWFkczlYeUJJdXFyb1I0YUQKeFJCd0xRS0I4UjJPSTBQOHAwclFqR1dHTGhOQnVvdkVFYzB0eHduT2JvdDgxSWRxSUNwZStPNzRZQXNKQWpFRgpqSHhPLzgvUU1ueHNqUnBNME1SK0ZkcWd3K2dIN2lzZGFBd0lKbko4dzJEQmlpT1VtamxtVGVZdk40cFdCMHdtCmlLN0VrM1l1MElqVnZhK0E3VXFLTmhRRWYzcEwwanRoZ0hDcnJIUHB2aFFZS25CUWxBWlk1RmNndDZ3QTBzUW8KSE4rTEo3SkFSY0Z2TWtmODlSUWlqTkozS1lxK1FTT1luL0R5dXZHaUpvaStDajhTTUIrWFZRPT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://Kubernetes-2.cabelas.corp:8443']
				kubernetesDeploy configs: 'whaleapp-deployment.yaml', kubeConfig: [path: ''], kubeconfigId: 'ravi_kubeconfig', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
				   
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
changeString = " - No new changes."
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
        changeString = " - No new changes."
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


		
	
