pipeline {
    agent any
    tools {
        jdk 'jdk8'
        maven 'maven3'
    }
    stages {
        stage("Compilation") {
            parallel 'Compilation': {
                sh "mvn clean install -DskipTests"
            }
        }
        stage("Tests and Deployment") {
            parallel 'Unit tests': {
                stage("Runing unit tests") {
                    try {
                        sh "mvn test"
                    } catch(err) {
                        step([$class: 'JUnitResultArchiver', testResults: 
                            '**/target/surefire-reports/TEST-*Test.xml'])
                        throw err
                    }
                    step([$class: 'JUnitResultArchiver', testResults: 
                        '**/target/surefire-reports/TEST-*Test.xml'])
                }
            }
        }
    }
}
