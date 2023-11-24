// pipeline {

//   agent any

//   stages {

//     stage('Checkout Source') {
//       steps {
//         git url:'https://github.com/samir282/Dummy_FullStack_Project.git', branch:'frontend'
//       }
//     }

//     stage('Deploy App') {
//       steps {
//         script {
//           kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubeconfig1")
//         }
//       }
//     }

//   }

// }



pipeline {
    agent any

    environment {
        KUBECONFIG = credentials('kubeconfig1')
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
        NAMESPACE = 'kasplo-app' // Set your desired namespace
        IMAGE_NAME = 'kasplo-frontend'
        GIT_REPO_URL = 'https://github.com/samir282/Dummy_FullStack_Project.git'
        GITHUB_CREDENTIALS = credentials('github')
        GIT_BRANCH = 'frontend'
    }

    stages {
        stage('Checkout Source') {
            steps {
               script {
                    withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_PASSWORD')]) {
                        git url: GIT_REPO_URL, branch: GIT_BRANCH, credentialsId: 'github'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        def customImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        def customImage = docker.image("${IMAGE_NAME}:${BUILD_NUMBER}")
                        customImage.push()
                    }
                }
            }
        }

        stage('Deploy App') {
            steps {
                script {
                    // Load the Kubernetes configuration from the provided kubeconfig credential
                    def kubeconfig = credentials('kubeconfig1')
                    withKubeConfig([credentialsId: 'kubeconfig1', kubeconfigFileVariable: 'KUBECONFIG']) {
                        // Use Kubernetes specific Groovy commands to deploy the app
                        kubernetesDeploy(configs: "deployment.yaml")
                    }
                }
            }
        }
    }
}

