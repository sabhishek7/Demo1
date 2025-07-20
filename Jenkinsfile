pipeline {
    agent any

    parameters {
        booleanParam(name: 'MERGE_APPROVED', defaultValue: false, description: 'Check this if the PR is approved for merging into main')
    }

    environment {
        GIT_USER = "sabhi-bot"
        GIT_EMAIL = "jenkins@example.com"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building code from branch: ${env.BRANCH_NAME}"
                // Insert build commands if needed
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Insert test commands if needed
            }
        }

        stage('Approval Check') {
            when {
                branch pattern: "Demo-B", comparator: "REGEXP"
            }
            steps {
                script {
                    if (!params.MERGE_APPROVED) {
                        error("Merge not approved. Please check 'MERGE_APPROVED' and rebuild.")
                    }
                }
            }
        }

        stage('Merge to Main') {
            when {
                allOf {
                    branch pattern: "Demo-B", comparator: "REGEXP"
                    expression { return params.MERGE_APPROVED }
                }
            }
            steps {
                script {
                    sh '''
                    git config user.email "${GIT_EMAIL}"
                    git config user.name "${GIT_USER}"
                    git fetch origin main
                    git checkout main
                    git merge origin/Demo-B -m "Merged Demo-B into main via Jenkins pipeline"
                    git push origin main
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline run complete."
        }
    }
}
