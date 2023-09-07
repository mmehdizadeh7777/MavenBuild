#!/usr/bin/env groovy
//@Library('jenkins-shared-library')_
library identifier: 'jenkins-shared-library@develop', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://github.com/mmehdizadeh7777/jenkins-shared-library.git',
    credentialsId: 'github_creds'
    ])

pipeline {
    agent any

	tools {
		maven 'local-maven'
	}
	parameters {
		booleanParam(name: 'executeTest', defaultValue: true, description: '')
	}

    stages {
        stage('increment version') {
                    steps {
                        script {
                            echo 'incrementing app version...'
                            sh 'mvn build-helper:parse-version versions:set \
                                -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                                versions:commit'
                            def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                            def version = matcher[0][1]
                            env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                        }
                    }
                }

        stage('build') {
            steps {
                script {
                    sh 'mvn clean package'
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
                     buildimage "mmehdizadeh7777/maven-example:${IMAGE_NAME}"
                  }
	        }
        }
        stage('deploy') {
                    steps {

                        script {
                            echo 'deploying docker image to EC2...'
                            //def dockerCmd = 'docker run -p 8080:8080 -d mmehdizadeh7777/maven-example:${IMAGE_NAME}'
                           def dockerCmd = "docker-compose -f docker-compose.yml up --detach"
                           sshagent(['ec2-key-pair']) {
                              sh "scp docker-compose.yml ubuntu@54.173.115.81:/home/ubuntu"
                              sh "ssh  -o StrictHostKeyChecking=no ubuntu@54.173.115.81 ${dockerCmd}"
                           }
                        }
                    }
                }
    }
}
