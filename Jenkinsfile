pipeline {
	agent { label 'master' } 
	options {
        timeout(time: 1, unit: 'DAYS')
        retry(2)
    }

	stages {
		stage('Installation') {
			steps {
				sh 'curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -'
                		sh 'sudo apt install -y nodejs'
			}
		}
				
			
		stage('npm install to run in parallel') {
			steps {
				sh 'npm install'
				echo 'Installed npm'
			}
		}

				
		stage('Deliver') {
			when { branch 'master' }

			steps {
            			input 'Proceed to Deploy'
            			
                		sh 'npm run start:dev'
                		echo 'Deployed.'
            		}
        	}
	
	}

	post {
		always {
			echo 'I will be executed always, P.S. I am in declarative pipeline\'s always block.'
		}

		changed {
			echo 'I am executed only when there is a change w.r.t the previous build.'
		}

		failure {
			echo 'I am executed only when there is a failure.'
		}

		success {
			echo 'I am executed only when there is a success.'
		}
		aborted {
			echo 'I am executed only when the build is aborted/ stopped manually.'
		}
	}
}

