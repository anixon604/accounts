killall_jobs()

pipeline {
    agent {
        docker {
            image 'azul/zulu-openjdk-debian:8'
            args '-u ${USER}'
        }
    }
    options { timestamps() }

    environment {
        EXECUTOR_NUMBER = "${env.EXECUTOR_NUMBER}"
    }

    stages {
        stage('Unit Tests') {
            steps {
                sh "./gradlew test --info"
            }
            post {
                always {
                    junit '**/build/test-results/**/*.xml'
                }
            }
        }
    }
}

@NonCPS
def killall_jobs() {
    def jobname = env.JOB_NAME
    def buildnum = env.BUILD_NUMBER.toInteger()

    def job = Jenkins.instance.getItemByFullName(jobname)
    for (build in job.builds) {
        if (!build.isBuilding()) {
            continue;
        }

        if (buildnum == build.getNumber().toInteger()) {
            continue
        }

        echo "Killing task = ${build}"
        build.doStop();
    }
}