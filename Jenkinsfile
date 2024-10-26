#!/user/bin/env groovy
@Library('jenkins-shared-library')
def gv

pipeline {   
    agent any
    tools {
    maven 'maven-3.9'
    }
    environment {
        NEW_VERSION = '1.3.0'
    }
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description:'')
        booleanParam(name:'executeTests', defaultValue: true, description:'')
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script {
                    echo 'testing the application...'
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    buildImage()
                }
            }
        }
        stage("deploy") {
             when {
                expression {
                    BRANCH_NAME == 'master'
                }
             }
             steps {
                script {
                    env.ENV = input message: "Select environment to deploy to", ok: "Done", parameters: [choice(name: 'ONE', choices: ['dev', 'staging', 'production'], description:'')]
                    echo 'deploying the application...'
                    echo "deploying version ${params.VERSION}"
                    echo "deploying to ${ENV}"
                }
            }
        }
    }
} 
