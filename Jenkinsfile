#!/usr/bin/env groovy
@Library('jenkins-shared-library')_
pipeline {
    agent any
	
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
