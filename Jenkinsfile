pipeline {
    agent any

    environment {
        // Define environment variables here
        MY_ENV_VAR = 'Custom Value'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clone the Git repository's master branch
                    def gitRepoUrl = 'https://github.com/nithinmakkena22/jenkins-tomcat.git'

                    checkout([$class: 'GitSCM', 
                        branches: [[name: '*/master']], 
                        userRemoteConfigs: [[url: gitRepoUrl]], 
                        extensions: [[$class: 'CleanBeforeCheckout'], [$class: 'CloneOption', noTags: false, shallow: true, depth: 1]]
                   ])


            }
        }
}
stage('Build Application') {
            steps {
                sh 'mvn -f java-tomcat-sample/pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts...."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
stage('Upload War To Nexus'){
            steps{
                script{
                          nexusArtifactUploader artifacts: [[artifactId: 'MAVEN-PROJECT-2024-09-05-BUILD02', classifier: '', file: '/var/lib/jenkins/workspace/jenkins-5/java-tomcat-sample/target/MAVEN-PROJECT-2024-09-05-BUILD02-0.0.1.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.example', nexusUrl: '172.31.28.126:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'tomcat-5', version: '0.0.1'
                
                    }
            }
        }

    }            
}
