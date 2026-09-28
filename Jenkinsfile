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
              bat 'docker stop portfolio-container || true'
              bat 'docker rm portfolio-container || true '
              bat 'docker run -d -p 8081:80 --name portfolio-container hydrauser/portfolio'
            }
        }
    }

}