node{
    def root = tool name: 'Maven1', type: 'maven'
    
    ws("${HOME}/agent/jobs/${JOB_NAME}/builds/${BUILD_ID}/") {               

            stage('Initialize') {
               sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        
        stage('Checkout') {
                
                    git(
                        url: 'https://github.com/ShrutiMaske/Demo.git',
                        branch: "master"
                    )
                
            }


            withCredentials([usernamePassword(credentialsId: '7ff51d39-65f2-4ae4-93b0-14505d18750e', 
                passwordVariable: 'IBM_CLOUD_DEVOPS_CREDS_PSW', usernameVariable: 'IBM_CLOUD_DEVOPS_CREDS_USR')]) {
                def gitCommit = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
                stage('Build') {
                   withEnv(["GIT_COMMIT=${gitCommit}",
                         'GIT_BRANCH=master',
                         "GIT_REPO=https://github.com/xunrongl/DemoDRA-1"]) {
                    try {
                         sh 'mvn clean install' 

                        // use "publishBuildRecord" method to publish build record
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", duration: 1, hostName: "local-dash.gravitant.net", serviceName: "Hello"
                    }
                    catch (Exception e) {
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", duration : 11, hostName: "local-dash.gravitant.net", serviceName: "Hello"
                    }
                    
                    
            }
            }
                stage('Unit Test and Code Coverage') {
                   // junit 'target/surefire-reports/**/*.xml'
                    // use "publishTestResult" method to publish test result
//publishTestResult type:'unit', fileLocation: '/var/jenkins_home/workspace/Jenkins-Github/simpleTest.json'
                    publishTestResult fileLocation: './simpleTest.json', type: "unit", serviceName: "ServiceNameTest", hostName: "local-dash.gravitant.net", resultType: "unit"
                } 
}
    }
}
