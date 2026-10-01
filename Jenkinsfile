pipeline {
agent any

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

    stage('Docker Deploy') {
        steps {
            sh 'docker stop portfolio-container || true'
            sh 'docker rm portfolio-container || true'
            sh 'docker run -d -p 8081:80 --name portfolio-container hydrauser/portfolio'
        }
    }

    stage('Push to Docker Hub') {
        steps {
            withCredentials([
                usernamePassword(
                    credentialsId: 'hydrauser',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )
            ]) {
                sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                sh 'docker push hydrauser/portfolio'
            }
        }
    }
}

}