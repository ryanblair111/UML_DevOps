pipeline {
    agent {
        label 'docker-agent'
    }
    
    stages {
/*        stage('Checkout code and prepare environment') {
            steps {
                git url: 'https://github.com/ryanblair111/UML_DevOps.git', 
                branch: 'main'
                        sh """
                            cd Chapter08/sample1
                            chmod +x gradlew
                        """
            }
        }*/

        stage ('Run CodeCoverage test if main branch') {
            when { branch 'main'}
            steps {
                sh """
                cd Chapter08/sample1
                chmod +x gradlew
                ./gradlew jacocoTestCoverageVerification
                """
            }
        }

        stage ('Run tests other than CodeCoverage on feature branches') {
            when { branch '*feature*' }
            steps {
                sh """
                cd Chapter08/sample1
                chmod +x gradlew
                ./gradlew test
                ./gradlew jacocoTestReport
                ./gradlew checkstyleMain
                """
                publishHTML (
                    target: [
                        reportDir: 'Chapter08/sample1/build/reports/tests/test',
                        reportFiles: 'index.html',
                        reportName: "JaCoCo Report"
                    ]
                )
                publishHTML (
                    target: [
                        reportDir: 'Chapter08/sample1/build/reports/checkstyle',
                        reportFiles: 'main.html',
                        reportName: "JaCoCo Checkstyle"
                        ]
                )
            }
        }

        stage ('Fail build if other branch') {
            when {
                not {
                    anyOf {
                        branch 'main'
                        branch '*feature*'
                    }
                }
            }
            steps {
                error("Build failed because it is not a main or feature branch")
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
    }
}
