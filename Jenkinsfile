pipeline {
    agent any

    stages {
        stage('Hello') {
            agent {
                docker {
                    image "amazon/aws-cli"
                    args "--entrypoint=''"
                    reuseNode true
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-alpha-23-jenkins-01', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    aws s3 ls
                    aws ecs register-task-definition --cli-input-json file://aws/aws-ecs-001-td-simple-app.json
                }
            }
        }
    }
}