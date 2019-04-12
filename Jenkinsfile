/**
 * Jenkinsfile
 */
pipeline {
    agent any
    /*
    agent {
      node { label 'linux' }
    }
    */

    /*
    options {

        disableConcurrentBuilds()
        timeout(time: 3, unit: 'MINUTES')
        timestamps()

        buildDiscarder(
            // disable automatic checkout of your repository
            skipDefaultCheckout(true)
            // Only keep the 3 most recent builds
            logRotator(numToKeepStr:'3'))
    }
    */

    environment {
    }

    stages {
        /*
        stage ('Code pull') {
            steps {
                checkout scm
            }
        }
        */
        stage('Build environment') {
            steps {
                sh """
                    [ -d venv ] && rm -rf venv
                    virtualenv --python=python2.7 venv
                    . venv/bin/activate
                    pip install pytest-runner pylint check-manifest codacy-coverage
                    pip install -U -r requirements.txt
                    pip install -U -r test-requirements.txt
                """
            }
        }
        stage ('Check style') {
                    steps {
                        // command || true continue execution when command fails
                        sh """
                            . venv/bin/activate
                            [ -d report ] || mkdir report
                        """
                        echo "Check manifest"
                        sh """
                            check-manifest | tee report/check-manifest.log || true
                        """
                        echo "flake8"
                        sh """
                          tox -e flake8 | tee report/flake8.log || true
                        """
                        sh """
                            # make pylint | tee report/pylint.log || true
                        """
                    }
        }
        stage('Test') {
            steps {
                sh """
                    rm -rf build
                    . venv/bin/activate
                    pip install -e .[test]
                    py.test -v --cov=swimlane --cov-report=xml

                    # Need CODACY_PROJECT_TOKEN
                    # python-codacy-coverage -r coverage.xml

                    # Check python version
                    python --version

                    # Check tox version
                    tox --version

                    # List all available environments
                    tox -a

                    # Run tox. Perhaps add '--skip-missing-interpreters' so the tests won't fail due to missing interpreters?

                    # tox -e py27,py34,py35,py36

                """
            }
        }
        stage('Build package') {
            steps {
                sh """
                    rm -rf dist
                    . venv/bin/activate
                    pip install --upgrade pip setuptools wheel
                    python setup.py sdist bdist_wheel
                    python2.7 offline_installer/build_installer.py
                    #python3.4 offline_installer/build_installer.py
                    #python3.5 offline_installer/build_installer.py
                    #python3.6 offline_installer/build_installer.py
                """
            }
        }
        stage ('Archive artifacts') {
            steps {
                sh """
                    . venv/bin/activate
                """
                archiveArtifacts artifacts: 'report/*'
            }
        }
        stage ('Cleanup') {
           steps {
               sh('Echo cleaning up...')
               sh('rm -rf venv')
           }
        }

        /*
        if (env.BRANCH_NAME == 'master') {
            // Option 1
            stage('Publish') {
                withEnv(['HOME=/dist']) {
                    sh """
                    . venv/bin/activate
                    pip install --upgrade pip setuptools wheel
                    python setup.py register -r swimlane
                    python setup.py sdist bdist_wheel upload -r swimlane
                    """
                }
            }

            // Option 2
            stage('Deploy') {
                environment {
                    TWINE = credentials('pypi-swimlane')
                }
                steps {
                    sh """
                    . venv/bin/activate
                    export TWINE_USERNAME=${TWINE_USR}
                    export TWINE_PASSWORD=${TWINE_PSW}
                    pip install --upgrade pip setuptools
                    pip install twine
                    twine upload dist/*
                    """
                }
            }
        }
        */

    }

    /*
    post {
      always {
        cleanWs(notFailBuild: true)
      }
      failure {
        slackSend(
          baseUrl: 'https://swimlane.slack.com/services/hooks/jenkins-ci/',
          botUser: true,
          channel: '#python-driver-test',
          color: 'danger',
          message: "WARNING: Python Swimlane Driver Deployment #${env.BUILD_NUMBER} FAILED: (<${env.BUILD_URL}|Details>)",
          teamDomain: 'swimlane',
          tokenCredentialId: 'slack-token')
      }
    }
    */
}
