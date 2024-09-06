pipeline {
    agent any
    
    environment {
        // Define the CodeDeploy application and deployment group names
        AWS_CREDENTIALS_ID = '4f3c7971-2511-4ad8-9fdc-f69d342d6760' // Jenkins credential ID for AWS
        S3_BUCKET = 'mywebapplications3bucket'
        S3_KEY = 'simple-web-app.zip'
        CODEDEPLOY_APPLICATION = 'WebApplication'
        CODEDEPLOY_DEPLOYMENT_GROUP = 'WebApp-dg'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the repository from Git
                git branch: 'main', url: 'https://github.com/Gulajkar-sham23/simple-web-app.git'
            }
        }

        stage('Package Application') {
            steps {
                // Zip the project files for CodeDeploy
                script {
                    sh 'zip -r ${S3_KEY} *'
                }
            }
        }

        stage('Upload to S3') {
            steps {
                // Upload the zip file to S3
                withAWS(credentials: "${AWS_CREDENTIALS_ID}", region: 'us-east-1') {
                    s3Upload bucket: "${S3_BUCKET}", file: "${S3_KEY}"
                }
            }
        }

        stage('Deploy to EC2 using CodeDeploy') {
            steps {
                // Trigger AWS CodeDeploy deployment
                withAWS(credentials: "${AWS_CREDENTIALS_ID}", region: 'us-east-1') {
                    script {
                        def deployment = awsCodeDeployDeploy(
                            applicationName: "${CODEDEPLOY_APPLICATION}",
                            deploymentGroupName: "${CODEDEPLOY_DEPLOYMENT_GROUP}",
                            s3LocationBucket: "${S3_BUCKET}",
                            s3LocationKey: "${S3_KEY}"
                        )
                        echo "Deployment ID: ${deployment.deploymentId}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}

