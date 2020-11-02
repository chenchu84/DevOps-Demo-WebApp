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
        
        
         stage ('DeployTest') {
            
            steps {
                    
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://3.23.98.14:8080')], contextPath: 'QAWebapp', war: '**/*.war'
            }
        }
        
        stage ('jFrogserver') {
            
            steps {
                
                rtServer (
                        id: 'artifactory',
                        url: 'https://devopsscriptedpipeline.jfrog.io/artifactory',
                        // If you're using username and password:
                        username: 'deploy',
                        password: 'Sai@feb2202',
                        // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
                        bypassProxy: true,
                        // Configure the connection timeout (in seconds).
                        // The default value (if not configured) is 300 seconds:
                        timeout: 300
                )
                    
             }
            
            steps {
                
                rtUpload (
    serverId: 'artifactory',
    spec: '''{
          "files": [
            {
              "pattern": "**/*.war",
              "target": "example-repo-local"
            }
         ]
    }''',
                    buildName: 'descriptivepipeline1'
 
   )
                    
             }
        }
        
       
        
         stage ('DeployProd') {
            
            steps {
                    
                    deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://3.137.192.69:8080')], contextPath: 'ProdWebapp', war: '**/*.war'
            }
        }
        
        stage ('SlackNotificationProd') {
            
            steps {
                
                slackSend channel: 'alerts', message: 'Prod Build Success', teamDomain: 'kv-workspacegroup', tokenCredentialId: 'slack2'
                    
             }
        }
        
       
        
    }
    
}
