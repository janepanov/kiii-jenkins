pipeline {
    agent any

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and push image') {
            when {
                expression { BRANCH_NAME == 'dev' }
            }
            steps {
                script {
                    def app = docker.build("janepanov/kiii-jenkins")
                    docker.withRegistry('https://registry.hub.docker.com', 'b42a7498-19a2-4611-8ed2-2f500f892774') {
                        if (BRANCH_NAME == 'dev') {
                            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                            app.push("${env.BRANCH_NAME}-latest")
                        } else {
                            echo "Changes were not made on the 'dev' branch, therefore we don't push the image to docker-hub."
                        }
                    }
                }
            }
        }
    }
}
