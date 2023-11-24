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
        // DOCKERHUB_USERNAME = "${DOCKERHUB_CREDENTIALS_USR}"
        // DOCKERHUB_PASSWORD = "${DOCKERHUB_CREDENTIALS_PSW}"
        NAMESPACE = 'kasplo-app'
        IMAGE_NAME = 'biswalashu/kasplo-frontend'
        GIT_REPO_URL = 'https://github.com/samir282/Dummy_FullStack_Project.git'
        GITHUB_CREDENTIALS = credentials('github')
        GIT_BRANCH = 'frontend'
    }

    stages {

        stage('Checkout Source') {
            steps {
                git url: GIT_REPO_URL, branch: GIT_BRANCH
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'echo doneeee'
                script {
                    docker.build("${IMAGE_NAME}:${TAG}", "-t ${IMAGE_NAME}:${TAG} .")
                }
            }
        }

        // stage('Push Docker Image') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
        //                 docker.image("${IMAGE_NAME}:${TAG}").push()
        //             }
        //         }
        //     }
        // }

        // stage('Deploy App') {
        //     steps {
        //         script {
        //             // Load the Kubernetes configuration from the provided kubeconfig credential
        //             def kubeconfig = credentials('kubeconfig1')
        //             withKubeConfig([credentialsId: 'kubeconfig1', kubeconfigFileVariable: 'KUBECONFIG']) {
        //                 // Use Kubernetes specific Groovy commands to deploy the app
        //                 kubernetesDeploy(configs: "deployment.yaml")
        //             }
        //         }
        //     }
        // }
    }
}

