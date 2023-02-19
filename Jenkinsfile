pipeline{
//    agent any
    agent {label 'worker'}
    options{
        buildDiscarder(logRotator(daysToKeepStr: '15'))
        disableConcurrentBuilds()
        timeout(time: 5, unit : 'MINUTES')
        retry(3)
    }
    triggers{
        cron('H */4 * * *')
        pollSCM('H * * * *')
    }
    parameters{
        string(name:'BRANCH', defaultValue: 'develop')
        choice(name:'Environment', choices: ['Dev', 'QA', 'Uat', 'Prod'])
        booleanParam(name: 'Notification', defaultValue: true)
    }
    stages{
        stage('Test Cases'){
            parallel{
                stage('linux test cases'){
                    steps{
                        sh "echo hello first stage"
                        sh "sleep 60"
                    }

                }
                stage('windows test cases'){
                    steps{
                        sh "echo hello second stage"
                        sh "sleep 60"
                    }
                }
            }
        }
        stage('Build and Push'){
            steps{
                sh "echo docker build and push"
            }
        }

    }
}
