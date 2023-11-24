pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/samir282/Dummy_FullStack_Project.git', branch:'frontend'
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubeconfig1")
        }
      }
    }

  }

}

\
