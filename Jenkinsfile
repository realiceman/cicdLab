pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment {
        artifactId = readMavenPom().getArtifactId()
        version    = readMavenPom().getVersion()
        name       = readMavenPom().getName()
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
                script {
                    def nexusRepo = version.endsWith("SNAPSHOT") ? "YoussefHarkati-SNAPSHOT" : "YhDevopsLab-RELEASE"
                
                    nexusArtifactUploader artifacts: [[artifactId: "${artifactId}", classifier: '', 
                                                    file: "target/${artifactId}-${version}.war", type: 'war']], 
                                                    credentialsId: 'f3a99197-b03a-4ea2-a17e-313e75d26ab0', 
                                                    groupId: 'com.youssefh', nexusUrl: '172.20.10.26:8081',
                                                        nexusVersion: 'nexus3', protocol: 'http', 
                                                    repository: "${nexusRepo}", version: "${version}"
                 }
            }
        }

         // Stage 4  : Print envs infos
        stage ('Print Env Variables'){
            steps {
                echo "ArtifactId is ... '${artifactId}'"
                echo "Version is ... '${version}'"
                echo "Name is... '${name}'"
            }
        }

        // Stage 5  : Deploy war to tomcat
        /*
        stage ('Deploying war to tomcat from ansible controller'){
            steps {
                echo "Deploying..."
                sshPublisher(publishers: 
                            [sshPublisherDesc(
                                configName: 'Ansible_Controller', 
                                transfers: [sshTransfer(
                                              cleanRemote: false, 
                                              execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                                              execTimeout: 120000
                                            )], 
                                usePromotionTimestamp: false, 
                                useWorkspaceInPromotion: false, 
                                verbose: false)
                            ])
            }
        }
        */

        // Stage 6  : Deploy war docker
        stage ('Deploying war to Docker'){
            steps {
                echo "Deploying..."
                sshPublisher(publishers: 
                            [sshPublisherDesc(
                                configName: 'Ansible_Controller', 
                                transfers: [sshTransfer(
                                              cleanRemote: false, 
                                              execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                                              execTimeout: 120000
                                            )], 
                                usePromotionTimestamp: false, 
                                useWorkspaceInPromotion: false, 
                                verbose: false)
                            ])
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