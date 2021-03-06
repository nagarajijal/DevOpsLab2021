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
        GroupId = readMavenPom().getGroupId()
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
        stage ('Sonarqube analysis'){
            steps {
                echo 'Source code pusblished for static code analysys......'
                withSonarQubeEnv('sonarqube'){
                    sh 'mvn sonar:sonar'
                } 
            }
        }

        // Stage3 : Push the artifact to nexus server
        stage ('Push to nexus'){
            steps {
                script{
                 
                def NexusRepo = Version.endsWith("SNAPSHOT") ? "NagarajDevOpsLab-SNAPSHOT" : "NagarajDevOpsLab-RELEASE"

                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '',
                file: "target/${ArtifactId}-${Version}.war", 
                type: 'war']],
                credentialsId: 'e204d783-a7b1-4a25-b991-8a9f5fb15dac', 
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.128:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
                }
            }
        }

        // Stage 4: Print the values
        stage ('Print the environment values'){
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "Name is '${Name}'"
                echo "Group ID is '${GroupId}'"
               }

        }
        
        // Stage5 : Deploying
        stage ('Deploy to Tomcat'){
            steps {
                echo "Deploying to tomcat server"
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible-controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/download-deploy.yaml -i /opt/playbooks/hosts',
                                execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        }

        // Stage6 : Deploying on Docker host
        stage ('Deploy to Docker'){
            steps {
                echo "Deploying to docker server"
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible-controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/download-deploy-docker.yaml -i /opt/playbooks/hosts',
                                execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        }
    }
}