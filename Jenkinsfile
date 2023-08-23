pipeline {
    agent any
	environment{
		NEW_VERSION = '1.0.3'
	}
	tools {
		maven 'local-maven'
	}
	parameters {
		booleanParam(name: 'executeTest', defaultValue: true, description: '')
	}

    stages {
        stage('build') {
            steps {
                echo 'building....'
		echo "building version ${NEW_VERSION}"
		//sh "mvn install"
            }
        }
	    stage('test') {
		    when{
			    expression {
				    params.executeTest
			    }
		    }
            steps {
                echo 'testing....'
            }
        }
	    stage('deploy') {
            steps {
                echo 'deploying....'
		withCredentials([usernamePassword(credentials:'github_creds', usernameVariable: USER, passwordVariable: PASS)])
		    {            
			    sh "docker login ${USER} ${PASS}"
		    }
	    }
        }
    }
}
