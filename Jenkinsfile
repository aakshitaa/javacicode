pipeline {

    agent any

    stages {
        stage("GIT Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/aakshitaa/javacicode.git'
            }
        }

        stage("UNIT TESTING") {
            steps {
                sh 'mvn test'
            }
        }

        stage("INTEGRATION TESTING") {
            steps {
                sh 'mvn verify -DskipUnitTesting'
            }
        }

        stage("BUILD") {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('STATIC CODE ANALYSIS') {
            steps {
                script {
                   withSonarQubeEnv(credentialsId: 'sonarqube1') {
                       sh 'mvn clean package sonar:sonar'
                   }
                }
            }
        }

//         stage('QUALITY GATE STATUS') {
//             steps {
//                 script {
//                     waitForQualityGate abortPipeline: false, credentialsId: 'tokenSonarqube'
//                 }
//             }
//         }

        stage("UPLOAD ARTIFACTS TO NEXSUS") {
            steps {
                script {
                    nexusArtifactUploader artifacts: [[artifactId: 'springboot', 
                    classifier: '', 
                    file: 'target/UPES.jar', 
                    type: 'jar']], 
                    credentialsId: 'nexus', 
                    groupId: 'com.example', 
                    nexusUrl: '13.126.70.84:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'maven1', 
                    version: '1.0.0'
                }
            }
        }
    }
}
