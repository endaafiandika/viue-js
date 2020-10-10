
pipeline {
    agent builderDocker
    agent any 

    parameters {
        booleanParam(name: 'RunTest', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: 'CICD', choices: ['CI', 'CICD'], description: 'Pick something')
    }
    stages {

        stage('build Project') {
            steps{
                nodejs("node12") {
                    sh 'yarn install'
                }
            }
        }

        stage('Build Docker Image') {
            steps{
                script {
                    CommitHash = sh (script : "git long -n 1 --pretty=format: '%H'", returnStdout: true)
                    builderDocker = docker.build("nopal/frontend:${CommitHash}")
                }
            }
        }

        stage('Run Testing') {
            when {
                expression {
                    params.RunTest
                }
            }
            steps{
                builderDocker.inside{
                    sh 'echo passed'
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    params.CICD == 'CICD'
                }
            }
            steps{
                echo 'testing.. '
            }
        }
    }
}