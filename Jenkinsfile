def approvalMap             // collect data from approval step

pipeline {
    agent none
	
	parameters {
        string(name: 'TASK', defaultValue: 'queues', description: 'Give Tasks')
        choice(name: 'ENV', choices: ['Dev-env.ini', 'Test-env.ini'], description: 'Pick something')

    }

    stages {
        stage('Stage 1') {
            agent none
            steps {
              mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL to build: ${env.BUILD_URL}", cc: '', 
		      charset: 'UTF-8', from: 'pathaknitin510@gmail.com', mimeType: 'text/html', replyTo: '', subject: "Approval Pending: Project name -> ${env.JOB_NAME}", to: "nitinpathak.orai@gmail.com";  
                timeout(60) {                // timeout waiting for input after 60 minutes
                    script {
                        // capture the approval details in approvalMap. 
                         approvalMap = input( 
                                        id: 'test', 
				        message: 'Hello', 
                                        ok: 'Proceed?', 
                                        parameters: [
                                            choice(
                                                choices: 'Dev-env.ini\nTest-env.ini', 
                                                description: 'Select an Env for this build', 
                                                name: 'Environment'
                                            ),
                                        string(
                                                defaultValue: '', 
                                                description: '', 
                                                name: 'Task'
                                            )
                                        ], 
                                        submitter: 'opsApprover', 
                                        submitterParameter: 'APPROVER')	
                            }
                }
            }
        }
        stage('Stage 2') {
            agent any

            steps {
                // print the details gathered from the approval
				//ENV = ${approvalMap['Environment']}
				//TASK = ${approvalMap['Task']}
                echo "This build was approved by: ${approvalMap['APPROVER']}"  	
                echo "This build is made using Env: ${approvalMap['Environment']}"
                echo "This build is using Task: ${approvalMap['Task']}"
            }
        }
		stage('Build') {
            agent any

            steps {
              sh  'ansible-playbook solace-vpn.yml --tags queues -i Dev-env.ini'
            }
        }
    }
} 				
