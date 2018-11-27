node {
  stage "Git fetch"
    git url: 'https://github.com/wongchunhung/pythonapp.git'
    sh 'git checkout master'
    sh 'git pull'

    echo 'Pulling... ${scm.branches[0].name}'

  stage "Docker Build"
    sh 'docker build -t chunha/pythonapp:0.1.${BUILD_NUMBER} .'

  stage "Unit test"
    echo "unit test..."

  stage "Integration test"
    echo "Integration test..."

  stage "Image Push"
    withCredentials([usernamePassword(credentialsId: 'harbor', passwordVariable: 'harborPassword', usernameVariable: 'harborUser')]) {
      sh "docker login -u ${env.harborUser} -p ${env.harborPassword} ingress.k8s-1.local"
      sh 'docker tag chunha/pythonapp:0.1.${BUILD_NUMBER} ingress.k8s-1.local/chunha/pythonapp:0.1.${BUILD_NUMBER}'
      sh 'docker push ingress.k8s-1.local/chunha/pythonapp:0.1.${BUILD_NUMBER}'
      }
}
stage('Deploy approval'){
    input "Deploy to prod?"
}
node {
    stage('Start Deployment'){
        build job: 'pythonapp_deploy_k8s_production', parameters: [[$class: 'StringParameterValue', name: 'BUILD', value: BUILD_NUMBER]]
    }
}
