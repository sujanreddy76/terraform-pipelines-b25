pipeline{
    agent {
        label 'terraform-slave'
    }
    parameters {
        choice(
            name: 'ACTION',
            choices: 'validate\ninit\nplan\napply\ndestroy'
        )
    }
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = "${WORKSPACE}/sa-key.json"
    }
    stages {
        stage('Setup GCP Auth') {
            steps {
                withCredentials([file(credentialsId: 'gcp-service-account-key', variable: 'SA_KEY')]) {
                        // some block
                        sh """
                           cp $SA_KEY $GOOGLE_APPLICATION_CREDENTIALS
                        """
                }
            }
        }
        //initialize terraform
        stage('init') {
            steps {
                echo "Initializing the terraform"
                sh "terraform init"
            }

        }

        //plan
        stage('plan') {
            when {
                expression {
                    params.ACTION == 'plan'
                }
            }
            steps {
                echo "Executing the plan for terraform"
                sh "terraform plan"
            }
        }

        //apply
        stage('apply') {
             when {
                expression {
                    params.ACTION == 'apply'
                }
            }
            steps {
                echo "Applying terraform infra"
                sh "terraform apply --auto-approve"
            }
        }

        //destroy
        stage('destroy') {
             when {
                expression {
                    params.ACTION == 'destroy'
                }
            }
            steps {
                echo "Destroying the terraform infra"
            }
        }
    }
    post {
        always {
            echo "************* Do clean the workspace *******************"
            cleanWs() //removes this pipeline workspace(each job,pipeline has its own workspace)
        }
    }

}