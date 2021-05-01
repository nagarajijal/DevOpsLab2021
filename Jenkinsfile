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

        // Stage3 : Push the artifact to nexus server
        stage ('Push to nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'ecommapp', classifier: '', file: 'target/com.nagarajLabs-0.0.3-SNAPSHOT', type: 'war']], credentialsId: 'e204d783-a7b1-4a25-b991-8a9f5fb15dac', groupId: 'ecommapp', nexusUrl: '172.20.10.128:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'NagarajDevOpsLab-SNAPSHOT', version: '0.0.3-SNAPSHOT'
            }
        }

        // Stage4 : Publish the source code to Sonarqube
        stage ('Deploy'){
            steps {
                echo ' Deploying......'
               }

            }
        }

        
        
    }

