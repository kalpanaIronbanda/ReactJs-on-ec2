pipeline {
    agent any
        parameters {
        string(name: 'bucketName', defaultValue: 'bucket name', description: 'bucket name')
        string(name: 'REMOTE_HOST', defaultValue: 'remote-host', description: 'Remote host')
        }

    stages {
        stage('Build') {
            steps {
                script{
                sh '''
                echo 'Building reactjs application...'
                rm -fr *.zip
                zip -r reactjs-${BUILD_NUMBER}.zip *
                aws s3 cp reactjs-${BUILD_NUMBER}.zip s3://${bucketName}/
                rm -fr *
                echo 'reactjs application built successfully!'
                '''
                }
            }
        }
        stage('Deploy') {
            steps {
                script{
                sh '''
                echo 'Deploying reactjs application...'
                ssh ec2-user@${REMOTE_HOST} "cd /home/ec2-user/ && sudo rm -rf *"
                aws s3 cp s3://${bucketName}/reactjs-${BUILD_NUMBER}.zip .
                scp reactjs-${BUILD_NUMBER}.zip ec2-user@${REMOTE_HOST}:/home/ec2-user/
                ssh ec2-user@${REMOTE_HOST} "cd /home/ec2-user/ && unzip reactjs-${BUILD_NUMBER}.zip && sudo rm -rf *.zip"
                rm -fr *.zip
                echo 'reactjs application deployed successfully!'
                '''
                }
            }
        }
        stage('Install') {
            steps {
                script{
                sh '''
                echo 'Installing npm...'
                ssh ec2-user@${REMOTE_HOST} "sudo yum install -y nodejs"
                ssh ec2-user@${REMOTE_HOST} "sudo npm install"
                echo 'Installed successfully...'
                '''
                }
            }
        }
        stage('Run'){
            steps{
                script{
                sh '''
                echo 'Starting reactjs application...'
                ssh ec2-user@${REMOTE_HOST} "sudo npm start &"
                echo 'reactjs application started successfully!'
                '''
                }
            }
        }
    }
}