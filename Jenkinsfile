/**
 * Jenkinsfile
 */
pipeline {
    agent any

    stages {
        stage('Build environment') {
            steps {
                sh """
                    ls
                    rm -rf venv
                    python --version
                """
            }
        }
    }
}
