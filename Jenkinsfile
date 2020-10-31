pipeline {
  agent any
  stages {
    stage('checkout SCM') { // Get code
      steps {
        // get code from our Git repository
        git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('Static Code Analysis') { // Get code
      steps {
        // get code from our Git repository
        def scannerhome = tool 'sonarqubescanner';
        withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonarqube') {
            sh """$(scannerhome)/bin/sonar-runner -D sonar.login=admin -D sonar.password=admin"""
        }
      }
    }
    stage('build') {
      steps {
      //sh 'mvn --version'
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('UI Test') {
      steps {
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('Performance Test') {
      steps {
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('Deploy To QA') {
      steps {
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('Store Artifact') {
      steps {
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('Deploy To Prod') {
      steps {
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
    stage('Sanity Test') {
      steps {
      git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'
      }
    }
  }
}
