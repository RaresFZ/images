pipeline {
    agent any

    environment {
        BUILD_DIR = "${WORKSPACE}"
        BINARY     = "compiled-app"
    }

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '25'))
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Dependencies') {
            steps {
                sh '''
                    set -euo pipefail
                    which gcc
                    gcc --version
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    set -euo pipefail
                    make
                '''
            }
        }

        stage('Smoke Test') {
            steps {
                sh """
                    set -euo pipefail
                    test -x ${BINARY}
                    ./${BINARY}
                """
            }
        }
    }

    post {
        always {
            sh 'make clean || true'
        }
        success {
            archiveArtifacts artifacts: "${BINARY}", fingerprint: true
        }
    }
}
