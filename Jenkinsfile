pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your GitHub repo
                git branch: 'main',
                    url: 'https://github.com/sriinathh/jenkins-cicd.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                // Use Windows batch commands to deploy to EC2
                bat '''
                REM Remove temp folder and create a new one on EC2
                ssh -i C:\\Users\\srinath\\.ssh\\jenkins_deploy -o StrictHostKeyChecking=no ec2-user@13.232.202.38 "rm -rf /tmp/test_dir && mkdir -p /tmp/test_dir"

                REM Copy all files from Jenkins workspace to EC2 temp folder
                scp -i C:\\Users\\srinath\\.ssh\\jenkins_deploy -o StrictHostKeyChecking=no -r "%WORKSPACE%\\*" ec2-user@13.232.202.38:/tmp/test_dir/

                REM Move files to nginx webroot and remove temp folder
                ssh -i C:\\Users\\srinath\\.ssh\\jenkins_deploy -o StrictHostKeyChecking=no ec2-user@13.232.202.38 "sudo rsync -a /tmp/test_dir/ /usr/share/nginx/html/ && sudo rm -rf /tmp/test_dir"
                '''
            }
        }
    }
}
