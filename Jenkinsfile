def gv

pipeline {
    agent any
    stages {
        stage("Copy files to ansible server") {
            steps {
                script {
                    echo "copying all necessary files to ansible control node..."
                    sshagent(['ansible-server-key']){
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@13.232.207.176:/root"

                        withCredentials([sshUserPrivateKey(credentialsId: "ec2-ssh-key", keyFileVariable: 'keyfile', usernameVariable: 'user' )]){
                            sh 'scp $keyfile root@13.232.207.176:/root/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage("execute ansible playbook"){
            steps{
                script{
                    echo"calling ansible playbook to configure ec2 instances"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "13.232.207.176"
                    remote.allowAnyHosts = true
                    withCredentials([sshUserPrivateKey(credentialsId: "ansible-server-key", keyFileVariable: 'keyfile', usernameVariable: 'user' )]){
                        remote.user = user
                        remote.identityFile = keyfile
                        sshCommand remote: remote, command: "ansible-playbook my-playbook.yaml"
                    }
                }
            }
        }
    }   
}