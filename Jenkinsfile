pipeline {
    agent {
        label 'docker-agent'
    }
    
    stages {
        stage('Checkout code and prepare environment') {
            steps {
                git url: 'https://github.com/ryanblair111/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git', 
                branch: 'master'
                        sh """
                            cd Chapter08/sample1
                            chmod +x gradlew
                        """
            }
        }
        stage ('Run tests and generate reports') {
            steps {
                sh """
                cd Chapter08/sample1
                ./gradlew test
                ./gradlew jacocoTestReport
                """
                publishHTML (
                    target: [
                        reportDir: 'Chapter08/sample1/build/reports/tests/test',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                        ]
                    )
            }
        }
        
        stage ('Run checkstyle and code coverage tests when java file changed') {
                when {
		    changeset "**/*.java"
		}		
		steps {
                    sh """
                    cd Chapter08/sample1
                    ./gradlew checkstyleMain
                    ./gradlew jacocoTestCoverageVerification

                    """
                }
            }

    }
        post {
	    failure {
	        echo 'Pipeline failure'
	    }
	    success {
		echo 'Pipeline ran perfectly'
	    }
            always {
                publishHTML (
                    target: [
                        reportDir: 'Chapter08/sample1/build/reports/checkstyle',
                        reportFiles: 'main.html',
                        reportName: "JaCoCo Checkstyle"
                        ]
                    )
                }
            }
}
