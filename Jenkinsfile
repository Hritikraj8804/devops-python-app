pipeline {
    agent {label 'blue'}
    
    stages {
        stage('code fork') {
            steps{
                echo 'code cloning'
                git url: 'https://github.com/Hritikraj8804/devops-python-app.git', branch: 'main'
                echo 'code clone complete'
            }
        }
        stage('code built') {
            steps{
                echo 'code building'
                sh "docker build -t python-app:latest ."
            }
        }
        stage('code push') {
            steps {
                echo 'pushing to docker hub'

                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    
                    docker tag python-app:latest $DOCKER_USER/python-app:latest
                    
                    docker push $DOCKER_USER/python-app:latest
                    '''
                }
            }
        }
        stage('code deploy') {
            steps{
                echo 'code deploying'
                sh "docker compose up -d"
            }
        }
    }
}
