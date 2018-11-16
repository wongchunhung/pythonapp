pipeline {
  agent none
  stages {
    stage('Initialize'){
      def dockerHome = tool 'myDocker'
      def mavenHome  = tool 'myMaven'
      env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${env.PATH}"
    }
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t chunha/pythonapp .'
      }
    }
  }
}
