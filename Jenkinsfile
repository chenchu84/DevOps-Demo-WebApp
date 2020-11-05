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
        
        stage ('StaticCodeAnalysis') {
            
            steps {
                withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonarqube') {
                sh 'mvn sonar:sonar -D sonar.login=admin -D sonar.password=admin'
                }
            }
        }
        
        stage ('Build') {
            
            steps {
                    sh 'mvn clean install'
            }
            
             post {
                 always {
                     jiraSendBuildInfo branch: 'https://tcs-devops-case-study.atlassian.net/browse/DC-1', site: 'tcs-devops-case-study.atlassian.net'
                      jiraComment body: 'Build is Successfull', issueKey: 'DC-1'
                 }
            
            }
            
            }
        
         //stage ('JiraNotification') {
            
           // steps {
             //      jiraComment body: 'Build is Success', issueKey: 'DC-1'
            //}
            //}
        
        
        stage ('DeployTest') {
            
            steps {
                    
                  deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://35.192.223.169:8080')], contextPath: 'QAWebapp', war: '**/*.war'
            }
            
             post {
                 always {
                     jiraSendDeploymentInfo environmentId: 'Test', environmentName: 'Test Environement', environmentType: 'Test', serviceIds: ['http://35.192.223.169:8080/QAWebapp'], site: 'tcs-devops-case-study.atlassian.net', state: 'successful'
                    jiraComment body: 'Test Deployment Successful', issueKey: 'DC-1'
                     slackSend channel: 'alerts', message: 'Test Deployment Success', teamDomain: 'friends-dover', tokenCredentialId: 'slackdevops'
                 }
             }
        }
        
       // stage ('SlackNotificationTest') {
            
         //   steps {
                
           //     slackSend channel: 'alerts', message: 'Test Deployment Success', teamDomain: 'friends-dover', tokenCredentialId: 'slackdevops'
                    
            // }
       // }
        
        //stage ('ArtifactoryBuild') {
            
          //  steps {
          //          sh 'mvn package'
           // }
       // }
        
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
     
      //   stage ('testbuild') {
            
        //    steps {
                
          //          sh 'mvn test -f functionaltest/pom.xml'
            
            //     }
        //}
    
    stage('UnitTest'){
        steps{
            sh 'mvn test -f functionaltest/pom.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
        }
    }
        
    //stage('performance-test'){
      //  steps{
            
        //   blazeMeterTest credentialsId: 'BlazeMeter', testId: '8656444.taurus', workspaceId: '683047'
        //}
     //}
        
         stage ('DeployProd') {
            
            steps {
                    
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://34.122.114.228:8080')], contextPath: 'ProdWebapp', war: '**/*.war'
            }
             
              post {
                 always {
                     jiraSendDeploymentInfo environmentId: 'Prod', environmentName: 'prod', environmentType: 'production', serviceIds: ['http://34.122.114.228:8080/ProdWebapp'], site: 'tcs-devops-case-study.atlassian.net', state: 'successful'
                     jiraComment body: 'Production Deployment Successful', issueKey: 'DC-1'
                     slackSend channel: 'alerts', message: 'Prod Deployment Success', teamDomain: 'friends-dover', tokenCredentialId: 'slackdevops'
                 }
             }
             
        }
        
      //  stage ('ProdBuild') {
            
        //    steps {
                
          //          sh 'mvn clean install -f Acceptancetest/pom.xml'
            
            //     }
        //}
    
    stage('SanityTest'){
        steps{
            sh 'mvn clean install -f Acceptancetest/pom.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\Acceptancetest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
        }
    }
        
       // stage ('SlackNotificationProd') {
            
         //   steps {
                
           //     slackSend channel: 'alerts', message: 'Prod Deployment Success', teamDomain: 'friends-dover', tokenCredentialId: 'slackdevops'
                    
             //}
       // }
        
       
        
    }
    
}
