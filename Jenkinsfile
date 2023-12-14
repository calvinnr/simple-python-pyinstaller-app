node {
    stage('Build') {
        // Define Docker image for the build stage
        docker.image('python:3.12.1-alpine3.19').inside {
            // Run build steps
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }

    stage('Test') {
        // Define Docker image for the test stage
        docker.image('databricksdocs/pytest:latest-py3.9').inside {
            // Run test steps
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
        }

        // Post-test actions
        post {
            always {
                // Publish JUnit test results
                junit 'test-reports/results.xml'
            }
        }
    }
}
