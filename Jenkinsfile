// Jenkinsfile - the pipeline Jenkins runs every time code is pushed to GitHub

pipeline {
    // Run on the Jenkins server itself (our EC2 instance)
    agent any

    environment {
        // Docker Hub username, used in the image names
        DH = 'sjcloud2024'
        // Tells kubectl where the k3s cluster config is
        KUBECONFIG = '/etc/rancher/k3s/k3s.yaml'
    }

    stages {
        // 1. Clone the latest code from GitHub
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // 2. Build both Docker images
        stage('Build images') {
            steps {
                sh 'docker build -t $DH/qaa-backend:latest ./backend'
                sh 'docker build -t $DH/qaa-frontend:latest ./frontend'
            }
        }

        // 3. Log in to Docker Hub and push the images
        //    (username/token come from the "dockerhub" credential in Jenkins)
        stage('Push images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                                  usernameVariable: 'DH_USER',
                                                  passwordVariable: 'DH_PASS')]) {
                    sh 'echo $DH_PASS | docker login -u $DH_USER --password-stdin'
                    sh 'docker push $DH/qaa-backend:latest'
                    sh 'docker push $DH/qaa-frontend:latest'
                }
            }
        }

        // 4. Deploy to Kubernetes: 2 backend + 2 frontend pods
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s/'
                // Restart the pods so they pull the new :latest images
                sh 'kubectl rollout restart deployment backend frontend'
                sh 'kubectl rollout status deployment backend'
                sh 'kubectl rollout status deployment frontend'
                sh 'kubectl get pods,svc'
            }
        }
    }
}
