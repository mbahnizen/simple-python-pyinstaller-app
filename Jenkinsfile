pipeline {
    agent {
        node {
            label 'docker'
        }
    }
    stages {
        stage('Build') {
            steps {
                script {
                    docker.image('python:3.10-alpine').inside {
                        sh 'pip install py_compile' //Pastikan py_compile terinstall
                        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('python:3.10-alpine').inside {
                        sh 'pip install pytest' //Pastikan pytest terinstall
                        sh 'pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
                    }
                }
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                script {
                    docker.image('cdrx/pyinstaller-linux:python2').inside {
                        sh 'pyinstaller --onefile sources/add2vals.py'
                    }
                }
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
