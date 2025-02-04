

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
                echo 'Building the code'
                sh "docker build -t portfolio-app ."
                sh "docker run -d -p 5173:5173 portfolio-app"
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing to DockerHub'
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
                    sh "docker tag hospital-app ${dockerHubUser}/portfolio-app:latest"
                    sh "docker push ${dockerHubUser}/portfolio-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the code'
                // Add your deployment logic here (e.g., deploying to a server or Kubernetes)
            }
        }
    }
}

