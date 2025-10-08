pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sriinathh/jenkins-cicd.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-deploy-key']) {
                    // Remove old temp folder and create new one
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@15.206.128.8 "rm -rf /tmp/test_dir && mkdir -p /tmp/test_dir"'

                    // Copy all files from Jenkins workspace to EC2 temp folder
                    sh 'scp -o StrictHostKeyChecking=no -r ${WORKSPACE}/* ec2-user@15.206.128.8:/tmp/test_dir/'

                    // Move files from temp folder to Nginx webroot
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@15.206.128.8 "sudo rsync -a /tmp/test_dir/ /usr/share/nginx/html/ && sudo rm -rf /tmp/test_dir"'
                }
            }
        }
    }
}
