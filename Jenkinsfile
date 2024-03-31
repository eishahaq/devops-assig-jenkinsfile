pipeline {
    agent any

    environment {
        // Define necessary environment variables
        REGISTRY = "us-central1-docker.pkg.dev/vocal-orbit-412510/iba-gar"
        IMAGE = "${REGISTRY}/todo-app:${env.BUILD_ID}"
        KUBECONFIG = '/path/to/kubeconfig' // Ensure this path is correct and secure
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git 'https://github.com/YourUsername/your-repo.git' // Replace with your repository URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${IMAGE} ."
                }
            }
        }

        stage('Push Image to Registry') {
            steps {
                script {
                    // Push the image to the configured container registry
                    sh "docker push ${IMAGE}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Update the Kubernetes manifests to use the built image
                    sh "sed -i 's|us-central1-docker.pkg.dev/vocal-orbit-412510/iba-gar/todo-app:latest|${IMAGE}|g' k8/web-application.yaml"

                    // Apply the Kubernetes manifests
                    sh "kubectl apply -f k8/load-balancer.yaml"
                    sh "kubectl apply -f k8/web-application.yaml"
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Clean up actions, if needed
                }
            }
        }
    }

    post {
        always {
            // Steps to perform after the pipeline, such as notifications
        }
    }
}
