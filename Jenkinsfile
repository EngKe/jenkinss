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
            withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DockerHubPassword', usernameVariable: 'DockerHubUser')]) {
        	sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPassword}"
            echo 'Logged!'
            }
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
                sh 'ls $HOME/.kube'
                sh 'kubectl config view'
                sh 'kubectl apply -f deployment.yaml '
                sh 'kubectl apply -f service.yaml '
                echo 'Deployement and Services created! '
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
