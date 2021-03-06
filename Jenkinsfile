pipeline {
	agent any

	parameters {
		string(name: 'PYTHON_INTERPRETER_PATH', defaultValue: '/usr/local/bin')
	}
	environment {
		KGX_GIT='https://github.com/NCATS-tangerine/kgx.git'
		PYTHONPATH='$WORKSPACE/kgx'
	}
	options {
		// using the timestamps plugin we can add timestamps to the console log
		timestamps()
	}

	stages {
		stage('KGX checkout') {
			steps {
				sh "cd $WORKSPACE"
				sh "pip3.7 install git+https://github.com/NCATS-Tangerine/kgx"
				script {
					if (!fileExists('$WORKSPACE/data')) {
						sh "mkdir $WORKSPACE/data"
					}
					if (!fileExists('$WORKSPACE/data')) {
						sh "mkdir $WORKSPACE/results"
					}
				}
			}
		}
		stage('Data download') {
			steps {
				sh "echo 'Download necessary data'"
			}
		}
		stage('Build the KG') {
			steps {
				sh "echo 'Parse the data using KGX'"
			}
		}
		stage('Last stage') {
			steps {
				sh "echo 'Persist the KG by saving to a Neo4j/Triple Store/file'"
			}
		}
	}
	post {
		always {
			// archive contents in results folder, only if the build is successful
			//archiveArtifacts artifacts: 'results/*', onlyIfSuccessful: true

			// delete all created directories
			deleteDir()

			// clean workspace
			cleanWs()
		}
	}
}
