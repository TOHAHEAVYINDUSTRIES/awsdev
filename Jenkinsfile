pipeline {
    agent any
    environment {
        ACCOUNTID = 'arn:aws:lambda:us-east-1:4989485945:function'
        BRANCH_PATTERN = 'refs/heads/master:refs/remotes/origin/master'
    }
    parameters {
        string(name: 'EMAILFNCT', defaultValue: 'email-function')
        string(name: 'validation', defaultValue: '')
    }
    stages {
        stage("Clean Workspace and Pull Latest Code") {
            steps {
                echo "Cleaning Workspace"
                cleanWs()
                echo "Workspace cleaned, pulling latest code..."
                checkout scm
            }
        }
        stage("Lint StepFunction Definition") {
            steps {
                expression {
                    echo "Validating JSON"
                    lintState = sh(script: 'python -m json.tool ${WORKSPACE}/aws-step-function.json > /dev/null', returnStdout: true)
                    env.validation = lintState
                }
            }
        }
        stage("Push Definition To AWS Step Functions") {
            when {
                expression {
                    env.validation == 0
                }
            }
            steps {
                echo "Stepfunction Definition is Valid"
                echo "Preflighting ${params.EMAIL}..."
            }
        }
    }
}