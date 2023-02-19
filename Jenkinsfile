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
                    environment{
                        REGION="us-east-1"
                    }
                    steps{
                        sh "echo ${REGION} first stage"
                    }
                }
                stage('windows test cases'){
                    steps{
                        sh "echo hello second stage"
                    }
                }
            }
        }
        stage('Build and Push'){
            steps{
                sh '''
                    set +xe
                    cd vote
                    DOCKER_BUILDKIT=0 docker build -t 803523228472.dkr.ecr.us-east-1.amazonaws.com/demo-c40:v${BUILD_NUMBER} .
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 803523228472.dkr.ecr.us-east-1.amazonaws.com
                    docker push 803523228472.dkr.ecr.us-east-1.amazonaws.com/demo-c40:v${BUILD_NUMBER}
                    '''
            }
        }

    }
    post{
        always{
            echo "its always block"
        }
        success{
            echo "its success block"
        }
        failure{
            echo "its failure block"
        }
    }
}
