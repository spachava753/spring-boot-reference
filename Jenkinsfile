pipeline {
    agent any
    tools {
        jdk 'jdk8'
        maven 'maven3'
    }
    stages {
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
}
