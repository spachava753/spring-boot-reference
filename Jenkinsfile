pipeline {
    agent any
    tools {
        jdk 'jdk8'
        maven 'maven3'
    }
    stages {
        stage("install") {
            steps {
                sh "mvn install"
            }
        }
        stage("Compilation") {
            parallel {
                stage('Compilation') {
                    steps {
                        sh "mvn clean install -DskipTests"
                    }
                }
            }
        }
        stage("Tests and Deployment") {
            parallel {
                stage("Runing unit tests") {
                    steps {
                        sh "mvn test"
                    }
                }
            }
        }
    }
}
