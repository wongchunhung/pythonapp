pipeline {
  agent none
  stages {
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t chunha/pythonapp:latest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'harborPassword', usernameVariable: 'harborUser')]) {
          sh "docker login -u ${env.harborUser} -p ${env.harborPassword} ingress.k8s-1.local"
          sh 'docker push ingress.k8s-1.local/chunha/pythonapp:latest'
        }
      }
    }
  }
}
