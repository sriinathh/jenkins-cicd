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
                // Use your private key directly instead of ssh-agent
                bat '''
                ssh -i C:\\Users\\srinath\\.ssh\\jenkins_deploy -o StrictHostKeyChecking=no ec2-user@15.206.128.8 "rm -rf /tmp/test_dir && mkdir -p /tmp/test_dir"
                scp -i C:\\Users\\srinath\\.ssh\\jenkins_deploy -o StrictHostKeyChecking=no -r "%WORKSPACE%\\*" ec2-user@15.206.128.8:/tmp/test_dir/
                ssh -i C:\\Users\\srinath\\.ssh\\jenkins_deploy -o StrictHostKeyChecking=no ec2-user@15.206.128.8 "sudo rsync -a /tmp/test_dir/ /usr/share/nginx/html/ && sudo rm -rf /tmp/test_dir"
                '''
            }
        }
    }
}
