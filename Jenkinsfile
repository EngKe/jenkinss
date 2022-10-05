pipeline {
    agent any

    stages {
        stage('Building Image') {
            steps {
                sh 'cd python | docker build -t kesginengin/pitonweb:lts .'
                echo 'Image Builded!'
            }
        }
        stage('Logging DockerHub') {
            steps {
                sh 'cat ~/pass.txt | docker login --username kesginengin --password-stdin'
                echo 'Logged!'
            }
        }
        stage('Pushing Image') {
            steps {
                sh 'docker push kesginengin/pitonweb:lts'
                echo 'Pushed!'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
                echo 'Deployement and Services created!'
            }
        }
        stage('Release') {
            steps {
                sh 'kubectl apply -f ingress.yaml'
                echo 'Released!'
            }
        }
    }
}
