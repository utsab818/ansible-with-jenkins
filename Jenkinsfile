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
                            sh "scp ${keyfile} root@13.232.207.176:/root/ssh-key.pem"
                        }
                    }
                }
            }
        }
    }   
}