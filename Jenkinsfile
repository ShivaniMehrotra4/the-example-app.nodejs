pipeline {
	agent any 
	options {
        timeout(time: 1, unit: 'DAYS')
        retry(2)
    }

	stages {
		stage('Installation to run in parallel') {
			parallel {
				stage('Install on slave 1') {
					agent {
						label ubuntu
					}
					steps {
						sh 'curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -'
                		sh 'sudo apt install -y nodejs'
					}
				}

				stage('Install on slave 2') {
					agent {
						label ubuntu
					}
					steps {
						sh 'curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -'
                		sh 'sudo apt install nodejs'
					}
				}
			}
		}

		stage('npm install to run in parallel') {
			parallel {
				stage('Install on slave 1') {
					agent {
						label ubuntu
					}
					steps {
						sh 'npm install'
						echo 'Installed npm'
					}
				}

				stage('Install on slave 2') {
					agent {
						label ubuntu
					}
					steps {
						sh 'npm install'
						echo 'Installed npm'
					}
				}
			}
		}

		// stage('Build on slave 2') {
		// 	agent {
		// 		label
		// 	}
		// 	steps {
		// 		sh 'npm install'
		// 		echo 'Installed npm'
		// 	}
		// }


		// stage("Proceed to production?") {
  //           agent none
  //           when { branch 'master' } 
  //           steps { proceedTo('production') }
  //       }

		stage('Deliver') {
			when { branch 'master' }

			steps {
            	input 'Proceed to Deploy'
            	ok 'I Agree.'
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

// def proceedTo(environment) {
//     def description = "Choose 'yes' if you want to deploy this build to " + 
//         "the ${environment} environment"
//     def proceed = 'no'
//     timeout(time: 4, unit: 'HOURS') {
//         proceed = input message: "Do you want to deploy the changes to ${environment}?",
//             parameters: [choice(name: "Deploy to ${environment}", choices: "no\nyes",
//                 description: description)]
//         if (proceed == 'no') {
//             error("User stopped pipeline execution")
//         }
//     }
// }