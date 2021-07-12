pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment {
        artifactId = readMavenPom().getArtficatId()
        version    = readMavenPom().getVersion()
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
        
        // Stage 3: Publish to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'YoussefHarkati', classifier: '', 
                                                   file: 'target/YoussefHarkati-0.0.4-SNAPSHOT.war', type: 'war']], 
                                                   credentialsId: 'f3a99197-b03a-4ea2-a17e-313e75d26ab0', 
                                                   groupId: 'com.youssefh', nexusUrl: '172.20.10.26:8081',
                                                    nexusVersion: 'nexus3', protocol: 'http', 
                                                   repository: 'YoussefHarkati-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }

         // Stage 4  : Print envs infos
        stage ('Print Env Variables'){
            steps {
                echo "ArtifactId is '${artifactId}'"
                echo "Version is '${version}'"
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