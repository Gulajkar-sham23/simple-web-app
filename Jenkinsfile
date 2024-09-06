pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Gulajkar-sham23/simple-web-app.git'
            }
        }
        stage('Upload to EC2') {
            steps {
                script {
                    sh 'scp -i -r Universalkey.pem your-app/ ubuntu@ubuntu:/var/www/html/'
                }
            }
        }
        stage('Deploy to CodeDeploy') {
            steps {
                script {
                    sh 'aws deploy create-deployment --application-name WebApplication --deployment-group-name WebApp-dg --revision "revisionType=GitHub,gitHubLocation={repository=your-repo,commitId=commit-id}"'
                }
            }
        }
    }
}
