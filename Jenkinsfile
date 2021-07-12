pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }
        
        // Stag 3: Publish to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'YoussefHarkati', classifier: '', 
                                                   file: 'target/YoussefHarkati-0.0.8.war', type: 'war']], credentialsId: 'f3a99197-b03a-4ea2-a17e-313e75d26ab0', 
                                                   groupId: 'com.youssefh', nexusUrl: '172.20.10.26:8081', nexusVersion: 'nexus3', protocol: 'http', 
                                                   repository: 'YhDevopsLab-SNAPSHOT', version: '0.0.8'
            }
        }

        

        // Stage4 : Publish the source code to Sonarqube
        // stage ('Sonarqube Analysis'){
        //     steps {
        //         echo ' Source code published to Sonarqube for SCA......'
        //         withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
        //              sh 'mvn sonar:sonar'
        //         }

        //     }
        // }

        
        
    }

}