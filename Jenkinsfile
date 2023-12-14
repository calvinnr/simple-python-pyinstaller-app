node {
    checkout scm
    stage('Build') {
	docker.image 'python:3-alpine'
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
    stage('Test') {
        docker.image 'qnib/pytest'
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        junit 'test-reports/results.xml'
    }
}
