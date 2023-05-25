pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('SonarScanner') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'Nexus', groupId: 'SampleWebApp', nexusUrl: 'ec2-13-59-197-88.us-east-2.compute.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-snapshot'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://18.116.19.135:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              
          }
            
        }
            
        }
} 
