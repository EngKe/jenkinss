pipeline {
    agent any

    stages {
        stage('Building Image') {
            steps {
                sh 'docker build -t kesginengin/pitonweb:lts python'
                echo 'Image Builded!'
            }
        }
        stage('Logging DockerHub') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
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
