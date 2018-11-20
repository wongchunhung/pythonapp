pipeline {
  agent none
  environment {
    BUILD_NUMBER = VersionNumber(projectStartDate: '1970-01-01', versionNumberString: '${BUILD_DATE_FORMATTED, "yyyyMMddHHmm"}', versionPrefix: '')
    }
  stages {
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t chunha/pythonapp:' + $BUILD_NUMBER + ' .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push chunha/pythonapp:latest'
        }
      }
    }
  }
}
