pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://ssuipathorchmultinode.eastus.cloudapp.azure.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "https://ssuipathorchmultinode.eastus.cloudapp.azure.com/"
	        UIPATH_ORCH_TENANT_NAME = "Default"
	        UIPATH_ORCH_FOLDER_NAME = "Shared"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      //version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
						  version: AutoVersion(),
	                      useOrchestrator: false,
						  traceLevel: 'None'
	        )
	            }
	        }
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
	

	         // Deploy Stages
	        stage('Deploy') {
	            steps {
	                echo "Deploying ${BRANCH_NAME}"
	                //UiPathDeploy (
	                //packagePath: "Output\\${env.BUILD_NUMBER}",
	                //orchestratorAddress: "${UIPATH_ORCH_URL}",
	                //orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                //folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                //environments: 'DEV',
	                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'ade18b82-5a45-44f6-aa49-cf5083bfde84'), 
					//traceLevel: 'None',
					//entryPointPaths: 'Main.xaml'
					
					UiPathDeploy (
					createProcess: true,
					credentials: UserPass('6c4202ac-a7be-420f-ac38-931701fd1677'),
					entryPointPaths: 'Main.xaml',
					environments: '',
					folderName: "${UIPATH_ORCH_FOLDER_NAME}",
					orchestratorAddress: "${UIPATH_ORCH_URL}",
					orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
					packagePath: "Output\\${env.BUILD_NUMBER}",
					traceLevel: 'None'
					)

	        //)
	            }
	        }
	

	

	         // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
				echo 'Always Executed!'
	            /* Clean workspace if success */
	            //cleanWs()
	        }
	    }
	

	}
