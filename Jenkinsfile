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
                        '**/target/surefire-reports/TEST-*Test.xml'])
                    throw err
                }
                step([$class: 'JUnitResultArchiver', testResults: 
                    '**/target/surefire-reports/TEST-*Test.xml'])
            }
        }
    }

    stage("Staging") {
        try {
            sh "pid=\$(lsof -i:9090 -t); kill -TERM \$pid "
                + "|| kill -KILL \$pid"
        } catch {
            withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
                sh 'nohup mvn spring-boot:run -Dserver.port=9090 &'
            }
        }
        withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
            sh 'nohup mvn spring-boot:run -Dserver.port=9090 &'
        }
    }
}
