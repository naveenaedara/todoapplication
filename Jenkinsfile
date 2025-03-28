pipeline{
    agent any
    environment{
        DOCKERHUB_CREDENTIALS=credentials('docker-hub-credentials')
        DOCKERHUB_USERNAME="naveenaedara1"
        IMAGE_NAME="todo-application-image"
        IMAGE_TAG="latest"
    }
    stages{
        stage('checkout code'){
            steps{
            git branch 'main', url 'https://github.com/naveenaedara/todoapplication.git'
        }
        }
    stage('build the code'){
        steps{
            sh "mvn clean package -DskipTests"
        }
    }
    stage('build docker image'){
        steps{
            script{
                sh "docker build -t $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG ."
            }
        }
    }
    stage('push docker image to dockerhub'){
        steps{
            script{
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                sh "docker push $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }
    stage('build application using docker compose'){
        steps{
            script{
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                sh '''
                docker compose version
                docker compose down -v
                docker compose up -d
                docker compose ps
                '''
            }
        }
    }
}
}

   
