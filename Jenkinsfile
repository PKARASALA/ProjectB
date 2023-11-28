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
            stage('Artifactory upload to Nexus'){
            
            steps{
                
                script{
                    
                    def readpomversion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readMavenPom.version.endswith{"SNAPSHOT"} ? "Web-application2-snapshot" : "Web-application2-release"
                    nexusArtifactUploader artifacts:
                    [ 
                        [
                            artifactId: 'springboot', 
                            classifier: '', file: 
                            'target/Uber.jar', type: 'jar'
                        ]
                    ], 
                    credentialsId: 'Nexus-Admin', 
                    groupId: 'com.example', 
                    nexusUrl: '54.206.122.108:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'Web-application2-release', 
                    version: "${readpomversion.version}"

                }
        }
}
}
}