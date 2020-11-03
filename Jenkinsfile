pipeline {
    
    agent any
    
    tools {
        maven 'Maven3'
    }
    
    
    stages {
        
        stage ('Checkout'){
            
            steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/chenchu84/DevOps-Demo-WebApp.git']]])
            }
            
        }
        
        stage ('Build') {
            
            steps {
                    sh 'mvn clean install'
            }
        }
        
         stage ('Code Analysis') {
            
            steps {
                withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonarqube') {
                sh 'mvn sonar:sonar -D sonar.login=admin -D sonar.password=admin'
                }
            }
        }
        
        
        
        
         stage ('DeployTest') {
            
            steps {
                    
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://35.188.158.168:8080')], contextPath: 'QAWebapp', war: '**/*.war'
            }
        }
        
        stage ('jFrogserver') {
            
            steps {
                
                rtServer (
                        id: 'artifactory',
                        url: 'https://devopsscriptedpipeline.jfrog.io/artifactory',
                        // If you're using username and password:
                        username: 'jenkins',
                        password: 'Sai@feb2202',
                        // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
                        bypassProxy: true,
                        // Configure the connection timeout (in seconds).
                        // The default value (if not configured) is 300 seconds:
                        timeout: 300
                )
                    
             }
        }
            
            stage ('jFrogserverupload') {
            
            steps {
                
                rtUpload (
    serverId: 'artifactory',
    spec: '''{
          "files": [
            {
              "pattern": "**/*.war",
              "target": "jenkins/WEBPOC/AVNCommunication/1.0/"
            }
         ]
    }''',
                    buildName: 'descriptivepipeline1',
                    buildNumber: '50'
 
   )
                    
             }
        }
     
         stage ('testbuild') {
            
            steps {
                
                    sh 'mvn clean install -f functionaltest/pom.xml'
            
                 }
        }
    
    stage('unittest'){
        steps{
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
        }
    }
        
    stage('performance-test'){
        steps{
            blazeMeterTest credentialsId: 'blazemeter', testId: '8656444.taurus', workspaceId: '683047'
        }
    }
        
         stage ('DeployProd') {
            
            steps {
                    
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://35.238.76.207:8080')], contextPath: 'ProdWebapp', war: '**/*.war'
            }
        }
        
        stage ('ProdBuild') {
            
            steps {
                
                    sh 'mvn clean install -f Acceptancetest/pom.xml'
            
                 }
        }
    
    stage('sanity-test'){
        steps{
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\Acceptancetest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
        }
    }
        
        stage ('SlackNotificationProd') {
            
            steps {
                
                slackSend channel: 'alerts', message: 'Prod Build Success', teamDomain: 'kv-workspacegroup', tokenCredentialId: 'slack2'
                    
             }
        }
        
       
        
    }
    
}
