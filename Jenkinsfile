node {
  stage "Git pull"
    git url: 'https://github.com/wongchunhung/pythonapp.git'
    sh 'git checkout master'
    sh 'git fetch https://github.com/wongchunhung/pythonapp.git'

    echo 'Pulling...' + ${env.BRANCH_NAME}

  stage "Docker Build"
    sh 'docker build -t chunha/pythonapp:0.1.${BUILD_NUMBER} .'

  stage "Docker Push"
    withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'harborPassword', usernameVariable: 'harborUser')]) {
      sh "docker login -u ${env.harborUser} -p ${env.harborPassword} ingress.k8s-1.local"
      sh 'docker tag chunha/pythonapp:0.1.${BUILD_NUMBER} ingress.k8s-1.local/chunha/pythonapp:0.1.${BUILD_NUMBER}'
      sh 'docker push ingress.k8s-1.local/chunha/pythonapp:0.1.${BUILD_NUMBER}'
      }

  stage "Start next job"

  build job: 'pythonapp_k8s_deploy', parameters: [[$class: 'StringParameterValue', name: 'BUILD', value: BUILD_NUMBER]]
}
