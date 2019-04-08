/**
 * Jenkinsfile
 */
pipeline {
    agent {
      node { label 'linux' }
    }

    options {
        /*
        disableConcurrentBuilds()
        timeout(time: 3, unit: 'MINUTES')
        timestamps()
        */
        buildDiscarder(
            // disable automatic checkout of your repository
            skipDefaultCheckout(true)
            // Only keep the 3 most recent builds
            logRotator(numToKeepStr:'3'))
    }
    environment {
    }
    stages {
        stage ('Code pull') {
            steps {
                checkout scm
            }
        }
        stage('Build environment') {
            steps {
                sh """
                    echo ${SHELL}
                    [ -d venv ] && rm -rf venv
                    python --version
                """
            }
        }
    }
}
