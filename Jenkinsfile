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
                sh 'kubectl apply -f deployment.yaml --context kind-pitonweb'
                sh 'kubectl apply -f service.yaml --context kind-pitonweb'
                echo 'Deployement and Services created! --context kind-pitonweb'
            }
        }
        stage('Release') {
            steps {
                sh 'kubectl apply -f ingress.yaml --context kind-pitonweb'
                echo 'Released!'
            }
        }
    }
}
