pipeline{
    agent any
    parameters {
        // Define parameters as they are defined in the Jenkins job configuration
        choice(name: 'environment', choices: ['dev', 'qa', 'prod'], description: 'Select the environment to deploy')

    }

    stages{
      stage('clean_workspace') {
        steps {
                // You can choose to clean workspace before build as follows
                cleanWs deleteDirs: true, notFailBuild: true
                checkout scm
               
            }
        }
        stage('build'){
            steps{
                // checkout to branch based on environment parameter
                script {
                    if (params.environment == 'dev') {
                        sh 'git checkout development'
                    } else if (params.environment == 'qa') {
                        sh 'git checkout qa'
                    }
                    //close if default branch is main or master
                    else {
                        sh 'git checkout main'
                    }
                }
               sh "echo 'Building the application for environment: ${params.environment}'"
               sh 'mvn clean package'
            }
        }
    }
}