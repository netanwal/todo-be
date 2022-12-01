pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f docker-pipeline-be -t image-builder --target builder .'
            }
        }
        stage('Delivery stage') {
            steps {
                sh 'DOCKER_BUILDKIT=1 docker build -f docker-pipeline-be -t netanelwalker/todo-be:jenkins-$BUILD_NUMBER --target delivery .'
            }
        }
        stage('Cleanup') {
            steps {
                echo "Cleanup-stage"
                sh 'docker system prune -f'
            }
        }
        stage('PUSH') {
            environment {
                dockerpwd = credentials('docker_pwd')
            }
            steps {
                    sh 'docker login -u netanelwalker -p ${dockerpwd}'
                    sh "docker push netanelwalker/todo-be:jenkins-$BUILD_NUMBER"
        }
    }
}
}
