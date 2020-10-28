pipeline {

   agent { docker { image 'maven:3.3.3' } }

   stages {

     stage('Static Code Analysis') { // Get code

       steps {

         // get code from our Git repository

         git 'https://github.com/chenchu84/DevOps-Demo-WebApp.git'

       }

     }

   }

 }
