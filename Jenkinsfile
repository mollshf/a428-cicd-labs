def remote=[:]
remote.name = 'ec2'
remote.user = 'ubuntu'
remote.host = 'ec2-47-129-246-98.ap-southeast-1.compute.amazonaws.com'
remote.identityFile = '/var/jenkins_home/.ssh/MSI-SERVER.pem'
remote.allowAnyHosts = true
pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                input message: 'Finished using the website? (Click "Proceed" to continue)'
            }
        }
        stage ('Deploy') {
            steps {
                sshCommand(remote: remote, command: "cd ~/a428-cicd-labs")
                sshCommand(remote: remote, command: "ls")
                sleep 5
                sshCommand(remote: remote, command: "git pull")
                sshCommand(remote: remote, command: "npm install")
                sshCommand(remote: remote, command: "./jenkins/scripts/deliver.sh")
                sleep 10
                sshCommand(remote: remote, command: "./jenkins/scripts/kill.sh")
            }
        }
    }
}
// node {
//     docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        
//         stage('Build') {
//             checkout scm
//             sh 'npm install'
//         }

//         stage('Test') {
//             sh './jenkins/scripts/test.sh'
//         }

//         stage('Manual Approval') {
//             input message: 'Lanjutkan ke tahap Deploy?? (Klik "Proceed" untuk melanjutkan)'
//         }
        
//         stage('Deploy') {
            // steps {
            //     sshCommand(remote: remote, command: "cd ~/a428-cicd-labs")
            //     sshCommand(remote: remote, command: "git pull")
            //     sshCommand(remote: remote, command: "npm install")
            //     sshCommand(remote: remote, command: "./jenkins/scripts/deliver.sh")
            //     sleep 10
            //     sshCommand(remote: remote, command: "./jenkins/scripts/kill.sh")
            // }
//         }
//     }
// }
