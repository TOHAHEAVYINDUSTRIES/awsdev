pipeline {
    agent any
    label "step-function-pipeline"
    environment {
        accountid = 'arn:aws:lambda:us-east-1:4989485945:function'
        parameters {
            eml = "email-function"
        }
    }
    stages {
        stage("Clean Workspace and Pull Latest Code") {
            steps {
                echo "Cleaning Workspace"
                cleanWs()
                echo "Workspace cleaned, pulling latest code..."
                checkout([$class: 'GitSCM',
                    branches: [[name: "origin/${BRANCH_PATTERN}"]],
                    doGenerateSubmoduleConfiguration: false,
                    extensions: [[$class: 'LocalBranch' ]],
                    submoduleCfg: [],
                    userRemoteConfigs: [[
                        credentialsId: 'tohaheavyindustries',
                        url: 'https://github.com/TOHAHEAVYINDUSTRIES/awsdev.git'
                    ]]
                ])
            }
        }
        stage("Lint StepFunction Definition") {
            steps {
                echo "Validating JSON"
                /* lintState = sh(script: 'jsonlint -d relaxed .\aws-stepfunction-definition.json', returnStdout: true) */
            },
            when {
                expression {
                    lintState == ""
                }
            }
            steps {
                echo "Stepfunction Definition is Valid"
            }
        }
    }
}