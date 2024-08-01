pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'Java'
    }

    environment {
        NEXUS_CREDENTIALS = credentials('nexuscred')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "demo-snapshot" : "demo-release"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'demo', 
                            classifier: '', 
                            file: "target/demo-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexuscred', 
                    groupId: 'in.javahome', 
                    nexusUrl: '13.234.76.245:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepoName, 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
