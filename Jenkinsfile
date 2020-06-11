#!groovy

pipeline {
    agent {
        label 'PACE Windows (Private)'
    }
    stages {
        stage('Checkout') { 
            steps {
                withCredentials([string(credentialsId: 'GitHub_API_Token', 
                            variable: 'api_token')]) {
                    bat '''
                        git config --local user.name "PACE CI Build Agent"
                        git clone https://pace-builder:%api_token%@github.com/pace-neutrons/document-test . &
                        git checkout %BRANCH_NAME%
                    '''
                }
            }
        }
        stage('Prepare') {
            steps {
                bat '''
                    git rm -r docs
                '''
            }
        }
        stage('Build') { 
            steps {
                bat '''
                    pip install sphinx &
                    pip install sphinx_rtd_theme &
                    make.bat html &
                    make.bat html
                '''
            }
        }
        stage('Deploy') { 
            steps {
                bat '''
                    git add docs
                    git commit -m "Document build from CI"
                    git push origin %BRANCH_NAME%
                '''
            }
        }
    }
}