pipeline {
    agent any
	environment{
		NEW_VERSION = '1.0.3'
		DOCKERHUB_CREDENTIALS = credentials('dockerhub')
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
		sh 'mvn package'
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
	    stage('build and push image') {
            steps {
                echo 'building image....'
		sh "docker build -t mmehdizadeh7777/maven-example:1.1 ."
		sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
		sh "docker push mmehdizadeh7777/maven-example:1.1"    
	    }
        }
    }
}
