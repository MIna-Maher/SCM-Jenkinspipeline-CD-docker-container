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
              stage('Transfering the project files to the production Web Server') {
              
            when {                 /*It's the Running stage condition, this stage will be running in case of meeting the condition*/
                
                branch 'master'   // only in master branch 
}
                 steps {
 withCredentials([usernamePassword(credentialsId: 'web_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(  //using of publish over ssh plugin to move source code files to Docker Web Server For production over ssh
                           failOnError: true,
                          continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'docker_production',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'output/archived-output.zip',
                                    //    removePrefix: 'output/',
                                        remoteDirectory: '/tmp',
                                    
execCommand: 'sudo unzip /tmp/output/archived-output.zip -d /tmp/output/ && sudo chown -R jenkins:jenkins /home/jenkins/Dockervolume && sudo rm -rf /home/jenkins/Dockervolume/index.html && sudo mv /tmp/output/index.html /home/jenkins/Dockervolume/ && sudo rm -rf /tmp/output/'
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
