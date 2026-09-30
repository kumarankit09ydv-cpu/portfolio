pipeline
{
    agent any
    stages
    {
        stage('git clone')
        {
           steps
           {
            git branch: 'main', url: 'https://github.com/kumarankit09ydv-cpu/portfolio.git'
           }
        }

        stage('Docker build')
        {
            steps
            {
               bat 'docker build -t hydrauser/portfolio .'
            }
        }

        stage('docker deploy')
        {
            steps
            {
              bat 'docker stop portfolio-container || exit 0'
              bat 'docker rm portfolio-container || exit 0 '
              bat 'docker run -d -p 8081:80 --name portfolio-container hydrauser/portfolio'
            }
        }

       stage('Push to Docker Hub') {
            steps
             {
              withCredentials([usernamePassword(credentialsId: 'hydrauser', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
               bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
               bat 'docker push hydrauser/portfolio'
        }
    }
      
    }

}