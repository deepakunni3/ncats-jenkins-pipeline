pipeline {
	agent any

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
				sh "git clone https://github.com/NCATS-Tangerine/kgx"
				sh "cd kgx && python3.7 setup.py install && cd .."
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
				// Download ontology files
				sh "wget http://purl.obolibrary.org/obo/mondo.owl -O data/mondo.owl"
				sh "wget https://raw.githubusercontent.com/obophenotype/human-phenotype-ontology/master/hp.owl -O data/hp.owl"
				sh "wget https://raw.githubusercontent.com/The-Sequence-Ontology/SO-Ontologies/master/so.owl -O data/so.owl"
				// Download datasets
				sh "wget https://data.monarchinitiative.org/ttl/hpoa.ttl -O data/hpoa.ttl"
				sh "wget https://data.monarchinitiative.org/ttl/hgnc.ttl -O data/hgnc.ttl"
			}
		}
		stage('Build the KG') {
			steps {
				sh "python3.7 $WORKSPACE/scripts/kgx_run.py"
			}
		}
		stage('Last stage') {
			steps {
				sh "ls -la results/"
			}
		}
	}
	post {
		always {
			// archive contents in results folder, only if the build is successful
			archiveArtifacts artifacts: '*/*', onlyIfSuccessful: true

			// delete all created directories
			deleteDir()

			// clean workspace
			cleanWs()
		}
	}
}
