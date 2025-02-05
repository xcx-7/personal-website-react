pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                echo 'Cloning the code'
                git url: "https://github.com/xcx-7/personal-website-react.git", branch: "master"
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Docker image'
                sh "docker build -t portfolio-app ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing to DockerHub'
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
                    sh "docker tag portfolio-app ${dockerHubUser}/portfolio-app:latest"
                    sh "docker push ${dockerHubUser}/portfolio-app:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes'
                sh "kubectl apply -f deployment.yaml"
                sh "kubectl apply -f service.yaml"
            }
        }
    }
}
