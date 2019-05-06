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
                sh "mvn install -Dmaven.test.failure.ignore=true"
                junit '**/target/surefire-reports/*.xml'

                // Archive the test results 
                step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
        }
        stage('SCA'){
                //Performing Static Code Analysis    
                sh "mvn sonar:sonar -Dsonar.host.url=http://172.27.59.108:9000 "
        }
              
        
        stage('Archive Artifact') {
                // Archive Artifact after build
                archiveArtifacts artifacts: 'target/EmployeeApplication*.war'
        }
        
 timeout(time:5, unit:'DAYS') {
         //Manual approval
         input message:'Approve deployment?', submitter: 'admin'
}
  //call(currentBuild.currentResult)
  slackSend color: "good", message: "Job: ${env.JOB_NAME} with buildnumber ${env.BUILD_NUMBER} was successful"

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



// this file is updated
