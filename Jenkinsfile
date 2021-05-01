pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupID = readMavenPom().getGroupId()
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
                nexusArtifactUploader artifacts: 
                [[artifactId: ${ArtifactId}, 
                classifier: '',
                file: 'target/ecommapp-0.0.4-SNAPSHOT', 
                type: 'war']],
                credentialsId: 'e204d783-a7b1-4a25-b991-8a9f5fb15dac', 
                groupId: ${GroupID}, 
                nexusUrl: '172.20.10.128:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'NagarajDevOpsLab-SNAPSHOT', 
                version: ${Version}
            }
        }

        // Stage 4: Print the values
        stage ('Print the environment values'){
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Name is '${Name}'"
                echo "Group ID is '${GroupID}'"
               }

            }
        

        // Stage5 : Publish the source code to Sonarqube
        stage ('Deploy'){
            steps {
                echo ' Deploying......'
               }

            }
        }

        
        
    }

