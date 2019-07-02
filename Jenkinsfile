node {
    stage 'Cloning the project'
    git 'https://github.com/spachava753/spring-boot-reference.git'
    tools {
        jdk 'jdk8'
        maven 'maven3'
    }
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
