// Jenkinsfiles are written in groovy which uses Java and C form comments
//CODE_CHANGES = getGitChanges()
def gv
pipeline {
    agent any
    parameters {
       // string(name: 'VERSION', defaultValue: ''. description: 'version to deploy on prod')
        choice(name: 'VERISON', choices: ['1.1.0', '1.1.1'], description: 'Pick a version')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Execute test if true')
    }
    tools {
        // tool name from Jenkins. Defaults maven, jdk, gradle
        maven 'Maven'
    }
    environment {
        VERSION = 'x.x'
        // Requires credentials and credentials binding plugin
        //SERVER_CREDENTIALS = credentials('ID')
    }

    stages {
        stage('init') {
            steps {
                script {
                    gv = load "pipeline.groovy"
                }
            }
        }
        stage('Build') {
            when {
                expression {
                   // BRANCH_NAME == 'feature/jenkins' && CODE_CHANGES == true
                    params.executeTests == true
                }    
            }
            steps {
                script { 
                    gv.buildApp()
                }
                ansible('Ansible2.9.6') {
                    sh 'ansible --version'
                }
            }
        }
        stage('Test') {
            when {
                expression {
                    BRANCH_NAME == 'feature/jenkins'
                }
            }
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}