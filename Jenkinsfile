pipeline {
    agent any
    environment {
        ACCOUNTID = 'arn:aws:lambda:us-east-1:4989485945:function'
        BRANCH_PATTERN = 'refs/heads/master:refs/remotes/origin/master'
    }
    parameters {
        string(name: 'EMAILFNCT', defaultValue: 'email-function')
    }
    stages {
        stage("Clean Workspace and Pull Latest Code") {
            steps {
                echo "Cleaning Workspace"
                cleanWs()
                echo "Workspace cleaned, pulling latest code..."
                /* checkout([$class: 'GitSCM',
                    branches: [[name: "origin/${BRANCH_PATTERN}"]],
                    extensions: [[$class: 'LocalBranch' ]],
                    submoduleCfg: [],
                    userRemoteConfigs: [[
                        credentialsId: 'tohaheavyindustries',
                        url: 'https://github.com/TOHAHEAVYINDUSTRIES/awsdev.git'
                    ]]
                ])
                */
                checkout scm
            }
        }
        stage("Lint StepFunction Definition") {
            when {
                expression {
                    sh 'pwd'
                    sh 'ls -l'
                    echo "${WORKSPACE}"
                    echo "Validating JSON"
                    lintState = sh(script: 'cat ${workspace}/aws-step-function.json | python -m json.tool', returnStdout: true)
                    echo "LintState is ${lintState}"
                    /* lintState == "" */
                }
            }
            steps {
                echo "Stepfunction Definition is Valid"
                echo "Preflighting ${params.EMAIL}..."
            }
        }
    }
}