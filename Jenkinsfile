pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-southeast-1'
    }

    stages {
        stage('ECS') {
            agent {
                docker {
                    image "amazon/aws-cli"
                    args "--entrypoint=''"
                    reuseNode true
                }
            }

            environment {
                AWS_ECS_CLUSTER = 'jenkins-cluster-01'
                AWS_ECS_SERVICE = 'td-simple-app-service-jjmo321w'
                AWS_ECS_TASK_DEFINITION = 'td-simple-app'
                CREDENTIAL_ID = 'aws-alpha-23-jenkins-01'
            }
            
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: env.CREDENTIAL_ID,
                        usernameVariable: 'AWS_ACCESS_KEY_ID',
                        passwordVariable: 'AWS_SECRET_ACCESS_KEY'
                    )
                ]) {
                    sh '''
                        aws --version
                        TASK_DEFINITION_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/aws-ecs-001-td-simple-app.json --query 'taskDefinition.revision' --output text)
                        aws ecs update-service --cluster $AWS_ECS_CLUSTER --service $AWS_ECS_SERVICE --task-definition $AWS_ECS_TASK_DEFINITION:$TASK_DEFINITION_REVISION
                        aws ecs wait services-stable --cluster $AWS_ECS_CLUSTER --services $AWS_ECS_SERVICE
                    '''
                }
            }
        }
    }
}