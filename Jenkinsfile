pipeline {
    agent any

    stages {
        stage('Build Image') {
            steps {
                sh '''
                cd client
                docker build -t yourdockerhub/netflix-client:latest .
                '''
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u yourdockerhub --password-stdin
                    docker push yourdockerhub/netflix-client:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f netflix-client || true
                docker run -d -p 3000:3000 --name netflix-client yourdockerhub/netflix-client:latest
                '''
            }
        }
    }
}
