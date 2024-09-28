node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        
        stage('Build') {
            checkout scm
            sh 'npm install'
        }

        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }

        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy?? (Klik "Proceed" untuk melanjutkan)'
        }
        
        stage('Deploy') {
                sshagent(credentials: ['ec2-001'])   {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i ~/.ssh/MSI-SERVER.pem ubuntu@ec2-13-229-134-251.ap-southeast-1.compute.amazonaws.com <<EOF
                    
                    cd ~/a428-cicd-labs

                    git pull

                    npm install

                    './jenkins/scripts/deliver.sh'
                    'sleep 60'
                    './jenkins/scripts/kill.sh'

                    exit
                    EOF
                    '''
                }
            }
        
    }
}
