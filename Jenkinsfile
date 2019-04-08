/**
 * Jenkinsfile
 */
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)

    stages {
        stage('Build environment') {
            steps {
              sh """
                  rm -rf venv
                  virtualenv --python=python2.7 venv
                  . venv/bin/activate
                  pip install --upgrade pip setuptools
                  pip install pytest-runner pylint check-manifest tox flake8
                  pip install -U -r requirements.txt
                  pip install -U -r test-requirements.txt
              """
            }
        }
    }
}
