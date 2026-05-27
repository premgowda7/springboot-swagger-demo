pipeline {
    agent any

    parameters {

        choice(
            name: 'environment',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select environment'
        )
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs deleteDirs: true, notFailBuild: true
            }
        }

        stage('Checkout Code') {
            steps {

                script {

                    def branchName = ""

                    if (params.environment == 'dev') {
                        branchName = "development"
                    }
                    else if (params.environment == 'qa') {
                        branchName = "qa"
                    }
                    else {
                        branchName = "main"
                    }

                    git branch: branchName,
                        credentialsId: 'github-cred',
                        url: 'https://github.com/premgowda7/springboot-swagger-demo.git'
                }
            }
        }

        stage('Build Application') {
            steps {

                sh "echo Building for ${params.environment}"

                sh 'mvn clean package'
            }
        }

        stage('Run Application') {
            steps {

                sh '''
                pkill -f java || true
                nohup java -jar target/*.jar > app.log 2>&1 &
                '''
            }
        }

        stage('Verify Application') {
            steps {

                sh 'sleep 15'

                sh 'curl http://localhost:8090'
            }
        }
    }
}
