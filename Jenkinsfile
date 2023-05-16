pipeline {

    agent any 
    
     triggers {
      cron('57 13 * * *')
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

     
        stage('Build') {
            steps {
                script {
                    // Get the branch name from the parameter or environment variable
                    def branchName = params.BRANCH_TO_BUILD ?: env.BRANCH_NAME
                    
                    // Clone the repository and checkout the specific branch
                    checkout([$class: 'GitSCM',
                        branches: [[name: "*/${branchName}"]],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [[$class: 'LocalBranch', localBranch: "${branchName}"]],
                        submoduleCfg: [],
                        userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                    ])

                    // Add your build steps here
                }
            }
        }
        stage(' Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            when {
                branch 'develop'
            }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
        }

    }   
}
