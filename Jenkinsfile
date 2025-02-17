pipeline {
    agent any

    environment {
        IMAGE_NAME = "bhargavmo14/react-k8s-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/Bhargav1470/React-K8S-App.git',
                credentialsId: 'github-credentials' // Replace with your actual credentials ID
            }
        }
            stage('Install npm') {
              steps {
                 script {
                       sh '''
                       if ! command -v node &> /dev/null; then
                       brew update
                       brew install node
                   else
                       echo "Node.js is already installed"
                   fi
                 '''
                }
            }
        }


        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'DockerHub-credentials', url: 'https://registry.hub.docker.com']) {
                    sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
                sh 'kubectl rollout status deployment/react-app'
            }
        }
    }
}
