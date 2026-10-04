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

    stage('Test') 
    {
       steps {
            sh 'docker run --rm hydrauser/portfolio ls /usr/share/nginx/html | grep index.html'
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

    stage('Deploy to Remote Server') {
        steps {
            sh 'ssh -i /var/lib/jenkins/ubuntuu.pem -o StrictHostKeyChecking=no ubuntu@3.7.252.69 "docker pull hydrauser/portfolio && docker stop portfolio-container || true && docker rm portfolio-container || true && docker run -d -p 80:80 --name portfolio-container hydrauser/portfolio"'
        }
    }
}

post {
    success {
        echo 'Build successful! Site deployed.'
    }
    failure 
    {
        echo 'Build failed! Check the logs.'
    }
}

}