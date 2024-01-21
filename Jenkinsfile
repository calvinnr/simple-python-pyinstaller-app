def branch = "master"
def repo = "git@github.com:calvinnr/simple-python-pyinstaller-app.git"
def cred = "calvin"
def dir = "~/simple-python-pyinstaller-app"
def server = "ubuntu@54.88.141.208"

pipeline {
    agent none
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:3.12.1-alpine3.19'
                }
            }
            steps {
		sshagent([cred]){
		sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
		cd ${dir}
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
		EOF"""
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
	stage('Manual Approval') {
	    steps {
		input 'Lanjutkan ke tahap Deploy?'
	    }
        }
        stage('Deploy') { 
            agent any
            environment { 
                VOLUME = '$(pwd)/sources:/src'
                IMAGE = 'cdrx/pyinstaller-linux:python2'
            }
            steps {
                dir(path: env.BUILD_ID) { 
                    unstash(name: 'compiled-results') 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'"
		    sleep time: 1, unit: 'MINUTES' 
                }
            }
            post {
                success {
                    archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
                    sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
		    exit
                }
            }
        }
    }
