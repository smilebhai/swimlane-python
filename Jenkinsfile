/**
 * Jenkinsfile
 */
pipeline {
    agent any

    options {
      disableConcurrentBuilds()
      timeout(time: 15, unit: 'MINUTES')
      timestamps()
    }

    environment {
      VERSION = '1.0.0'
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
