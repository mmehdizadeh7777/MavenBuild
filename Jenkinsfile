#!/usr/bin/env groovy
@Library('jenkins-shared-library')_
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
                script {
                    buildjar()
                }
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
                 script {
                     buildimage()
                  }
	        }
        }
    }
}
