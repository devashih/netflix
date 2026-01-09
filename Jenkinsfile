pipeline {
    agent any

    stages {
        stage('Build Image') {
            steps {
                sh '''
                cd frontend
                docker build -t yourdockerhub/netflix-clone:latest .
                '''
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u yourdockerhub --password-stdin
                    docker push yourdockerhub/netflix-clone:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f netflix || true
                docker run -d -p 3000:3000 --name netflix yourdockerhub/netflix-clone:latest
                '''
            }
        }
    }
}
