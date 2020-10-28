pipeline {
  agent any
  stages {
    stage('Static Code Analysis') { // Get code
      steps {
        // get code from our Git repository
        git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('build') {
      steps {
      sh 'mvn --version'
      }
    }    
  }
}

