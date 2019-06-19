node {
    stage 'Cloning the project'
    git 'https://github.com/spachava753/spring-boot-reference.git'

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
                        '**/target/surefire-reports/TEST-*UnitTest.xml'])
                    throw err
                }
                step([$class: 'JUnitResultArchiver', testResults: 
                    '**/target/surefire-reports/TEST-*UnitTest.xml'])
            }
        }
    }

    stage("Staging") {
        sh "pid=\$(lsof -i:9090 -t); kill -TERM \$pid "
            + "|| kill -KILL \$pid"
        withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
            sh 'nohup mvn spring-boot:run -Dserver.port=9090 &'
        }   
    }
}
