def gv

pipeline {   
    agent any
    environment {
        NEW_VERSION = '1.3.0'
    }
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description:'')
        booleanParam(name:'executeTests', defaultValue: true, description:'')
    }
    stages {
        stage("init") {
            script {
                gv = load "script.groovy"
            }
        }
        stage("build") {
            steps {
                script {
                    gv.buildApp()
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
        stage("deploy") {
            message "Select environment to deploy to"
            ok "Done"
            parameters {
                    choice(name: 'ENV', choices: ['dev', 'staging', 'production'], description:'')
            }
             steps {
                script {
                    echo 'deploying the application...'
                    echo "deploying version ${params.VERSION}"
                    echo "deploying to ${ENV}"
//                     withCredentials([
//                         usernamePassword(credentials: 'docker-hub', usernameVariable: USER, passwordVariable: PWD)
//                     ]) {
//                         sh "some script ${USER} ${PWD}"
//                     }
                }
            }
        }
    }
} 
