//Declarative pipeline syntax
pipeline {
agent any
/* Building Stage */    // CI Build stage 
       stages {                 
       stage('Archive build output') {
                      steps {
                      echo 'Running build automation'
                       
                        sh "mkdir -p output"
                         sh "zip output/archived-output.zip *"
                      archiveArtifacts artifacts: 'output/archived-output.zip' //output will be in output/archived-output.zip
            }
}
       }
}
