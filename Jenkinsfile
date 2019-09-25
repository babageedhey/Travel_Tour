pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                archiveArtifacts artifacts: 'dist/testbuild.zip'
            }
        }
        stage('DeployToTest') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'TestServer',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/testbuild.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo  rm -rf /home/geedhey/Travel_Tour/* && unzip /tmp/testbuild.zip -d /home/geedhey/Travel_Tour'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        
    }
}
