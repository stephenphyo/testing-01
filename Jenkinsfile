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
            environment {
                AWS_DEFAULT_REGION = 'ap-southeast-1'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-alpha-23-jenkins-01', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        TASK_DEFINITION_REVISION = $(aws ecs register-task-definition --cli-input-json file://aws/aws-ecs-001-td-simple-app.json --query 'taskDefinition.revision' --output text)
                        echo $TASK_DEFINITION_REVISION
                        aws ecs update-service --cluster jenkins-cluster-01 --service td-simple-app-service-jjmo321w --task-definition td-simple-app
                    '''
                }
            }
        }
    }
}