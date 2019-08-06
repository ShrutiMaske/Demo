pipeline {
    agent any
    environment {
        // You need to specify required environment variables first, they are going to be used for the following IBM Cloud DevOps steps
        //BM_CRED = '7ff51d39-65f2-4ae4-93b0-14505d18750e'
        IBM_CLOUD_DEVOPS_CREDS = credentials('7ff51d39-65f2-4ae4-93b0-14505d18750e')
       // IBM_CLOUD_DEVOPS_API_KEY = '7ff51d39-65f2-4ae4-93b0-14505d18750e'
    }
    tools {
        maven 'Maven1'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            environment {
                // get git commit from Jenkins
                GIT_COMMIT = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                GIT_BRANCH = 'master'
                GIT_REPO = 'GIT_REPO_URL_PLACEHOLDER'
            }
            steps {
                sh 'mvn clean install' 
            }
            post {
               // success {
                //    junit 'target/surefire-reports/**/*.xml' 
               // }
                success {
                    publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", duration: 1, hostName: "local-dash.gravitant.net", serviceName: "Hello"
                }
                failure {
                    publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", duration : 11, hostName: "local-dash.gravitant.net", serviceName: "Hello"
                }
            }
        }
    }
}
