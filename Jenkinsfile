pipeline {
    agent any
    stages {
        stage('test') {
            steps {
                echo "Testing the application"
                echo "Executing pipeline for branch $BRANCH_NAME"
            }
        }

        stage('build') {
            when {
                expression {
                    BRANCH_NAME == 'main'
                }
            }
            steps {
                echo "Building the application $BRANCH_NAME"
            }
        }

        stage('deploy to prod') {
            when {
                expression {
                    BRANCH_NAME == 'main'
                }
            }
            steps {
                echo "Deploying the application $$BRANCH_NAME"
                script {
                    def dockerCmd = 'docker run -p 3080:3080 -d tishadev/react-app'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@34.229.74.48 ${dockerCmd} " 

                    }
                }
            }
        }

        stage('deploy to dev') {
            when {
                expression {
                    BRANCH_NAME == 'dev'
                }
            }
            steps {
                echo "Deploying the application $$BRANCH_NAME"
                
            }
        }
    } 

    post {
        success {
            echo "Job is successful!"
        }
        failure {
            echo "Job is failed! Please check the logs for details."
        }
    }
}