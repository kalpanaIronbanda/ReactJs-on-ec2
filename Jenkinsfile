pipeline {
    agent any
        parameters {
        string(name: 'BUILD_NUMBER', defaultValue: '1', description: 'Build Number')
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
                zip -r reactjs-$BUILD_NUMBER.zip *
                aws s3 cp reactjs-$BUILD_NUMBER.zip s3://${params.bucketName}/
                #scp dependencies.sh ec2-user@{params.REMOTE_HOST}:/home/ec2-user/
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
                ssh ec2-user@{params.REMOTE_HOST} "cd /home/ec2-user/ && sudo rm -rf *"
                aws s3 cp s3://${params.bucketName}/reactjs-${params.BUILD_NUMBER} .
                scp reactjs-${params.BUILD_NUMBER} ec2-user@{params.REMOTE_HOST}:/home/ec2-user/
                ssh ec2-user@{params.REMOTE_HOST} "cd /home/ec2-user/ && unzip reactjs-${params.BUILD_NUMBER} && sudo rm -rf *.zip"
                rm -fr *.zip
                echo 'reactjs application deployed successfully!'
                '''
                }
            }
        }
    }
}