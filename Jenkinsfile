pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'master', url: 'https://github.com/PKARASALA/ProjectB.git'
                }
            }
        }
        stage('UNIT Testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration Testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven Build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static Code Analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'admin') {

                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
    }
            stage('Quality Gate Status'){
            
            steps{
                
                script{
                    
                    waitForQualityGate abortPipeline: false, credentialsId: 'admin'

                      }
        }
}
            stage('Artifact upload to nexus'){
            
            steps{
                
                script{
                    
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                    ], 

                    credentialsId: 'Nexus-Admin', 
                    groupId: 'com.example', 
                    nexusUrl: '13.236.117.82:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'Web-application2-release', 
                    version: '2.0.0'

                      }
        }
}
}
}