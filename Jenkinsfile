pipeline {
    agent any

    parameters {
        choice(
            name: 'DEPLOY_TARGET',
            choices: ['Local', 'Remote', 'Both'],
            description: 'Where to deploy?'
        )
    }

    stages {

        stage('Git Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/kumarankit09ydv-cpu/portfolio.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t hydrauser/portfolio .'
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm hydrauser/portfolio ls /usr/share/nginx/html | grep index.html'
            }
        }

        stage('Docker Deploy (Local)') {
            when {
                expression { params.DEPLOY_TARGET == 'Local' || params.DEPLOY_TARGET == 'Both' }
            }
            steps {
                sh 'docker stop portfolio-container || true'
                sh 'docker rm portfolio-container || true'
                sh 'docker run -d -p 8081:80 --name portfolio-container hydrauser/portfolio'
            }
        }

        stage('Push to Docker Hub (Remote)') {
            when {
                expression { params.DEPLOY_TARGET == 'Remote' || params.DEPLOY_TARGET == 'Both' }
            }
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'hydrauser',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push hydrauser/portfolio'
                }
            }
        }

    } 

    post {
        success {
            echo 'Build successful! Site deployed.'
        }
        failure {
            echo 'Build failed! Check the logs.'
        }
    }
}