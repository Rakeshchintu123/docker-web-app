pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/my-web-image"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Rakeshchintu123/docker-web-app.git'
            }
        }

      stage('Build Docker Image') {
    steps {
        bat 'docker build -t docker-web-app .'
    }
}
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop web || true
                    docker rm web || true

                    docker run -d --name web -p 80:80 $IMAGE_NAME
                '''
            }
        }
    }
}
