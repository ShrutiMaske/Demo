node{
   // def root = tool name: '3.8.8', type: 'mvn'
     //String jdktool = tool name: "jdk11", type: 'hudson.model.JDK'
   echo "Entering..."
    String jdktool = tool name: "Java_11", type: 'hudson.model.JDK'
    def mvnHome = tool name: 'jenkins_plugin_mvn'

    /* Set JAVA_HOME, and special PATH variables. */
    List javaEnv = [
        "PATH+MVN=${jdktool}/bin:${mvnHome}/bin",
        "M2_HOME=${mvnHome}",
        "JAVA_HOME=${jdktool}"
    ]
    
     withEnv(javaEnv) {
    stage ('Initialize') {
        sh '''
            echo "PATH = ${PATH}"
            echo "M2_HOME = ${M2_HOME}"
        '''
    }
    
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


            withCredentials([usernamePassword(credentialsId: '84048301-d1fb-4315-9e93-52405de7fdc7', 
                passwordVariable: 'IBM_CLOUD_DEVOPS_CREDS_PSW', usernameVariable: 'IBM_CLOUD_DEVOPS_CREDS_USR')]) {
                def gitCommit = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
                stage('Build') {
                   withEnv(["GIT_COMMIT=${gitCommit}",
                         'GIT_BRANCH=master',
                         "GIT_REPO=https://github.com/xunrongl/DemoDRA-1"]) {
                    try {
//                          sh 'mvn package' 
                           // junit 'target/surefire-reports/**/*.xml'
                        // use "publishBuildRecord" method to publish build record
//publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", duration: 1, hostName: "local-dash.gravitant.net", serviceName: "Serve"
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", hostName: "mcmp-di-release12.kyndryl-cloud.com", serviceName: "Shruti_test2"
  
                    }
                    catch (Exception e) {
//publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", duration : 11, hostName: "local-dash.gravitant.net", serviceName: "Serve"
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", hostName: "mcmp-di-release12.kyndryl-cloud.com", serviceName: "Shruti_test2"
  
                    }
                    
                    
            }
                    
            }
             
}
        withCredentials([usernamePassword(credentialsId: '9bbebcae-60a5-458c-9c3d-32595c60596e', 
                passwordVariable: 'IBM_CLOUD_DEVOPS_CREDS_PSW', usernameVariable: 'IBM_CLOUD_DEVOPS_CREDS_USR')]) {

                    stage('Unit Test and Code Coverage') {
                  
                    // use "publishTestResult" method to publish test result
//publishTestResult type:'unit', fileLocation: '/var/jenkins_home/workspace/Jenkins-Github/simpleTest.json'
                    publishTestResult fileLocation: 'target/surefire-reports/', type: "unit", serviceName: "Shruti_test1", hostName: "mcmp-di-release12.kyndryl-cloud.com", resultType: "junit"
                } 
                }
    }
}
}
