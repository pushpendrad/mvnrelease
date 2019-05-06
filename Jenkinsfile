node('JavaAgent') {
        slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} started"
  
        stage('Checkout') {
        // Cloning Repo
          git url: 'https://github.com/pushpendrad/emsdockerhub.git'
        }    
        
        stage('build') {
        // Building Code
           sh "mvn clean install -DskipTests"
        }
        stage('Test') {
        // Testing Code
          // runTests()
            sh "mvn install -Dmaven.test.failure.ignore=true"

  /* Archive the test results */
            step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        }
        stage('SCA'){
            sh "mvn sonar:sonar -Dsonar.organization=pushpendrad-github -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${env.SCA_TOKEN}"
        }
              
        
        stage('Archive Artifact') {
            // Archive Artifact after build
          archiveArtifacts artifacts: 'target/EmployeeApplicationSprint4-1.0-SNAPSHOT.war'
        }
 timeout(time:5, unit:'DAYS') {
    input message:'Approve deployment?', submitter: 'admin'
}
  //call(currentBuild.currentResult)
  slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
//slackSend baseUrl: 'https://jenkinsbuild.slack.com/services/hooks/jenkins-ci/', botUser: true, channel: '#jenkinsbuild', color: '#439FE0', failOnError: true, message: 'slack build failure', tokenCredentialId: 'jenkinsSlack'
}

def call(String buildResult) {
  if ( buildResult == "SUCCESS" ) {
    slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"
  }
  else if( buildResult == "FAILURE" ) { 
    slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was failed"
  }
  else if( buildResult == "UNSTABLE" ) { 
    slackSend color: "warning", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was unstable"
  }
  else {
    slackSend color: "danger", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} its resulat was unclear"	
  }
}


void runTests(def args) {
  /* Call the Maven build with tests. */
  mvn "install -Dmaven.test.failure.ignore=true"

  /* Archive the test results */
  step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
}


