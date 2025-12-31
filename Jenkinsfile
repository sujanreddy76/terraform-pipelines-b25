pipeline{
    agent {
        label 'terraform-slave'
    }
    parameters {
        choice (
            name: 'ENVIRONMENT',
            choices: ['dev', 'test', 'stage', 'prod'],
            description: 'Choose the environment to deploy'
        )
        choice(
            name: 'ACTION',
            choices: 'validate\ninit\nplan\napply\ndestroy'
        )
    }
    environment {
        GCS_BUCKET = "project-d2753c31-d4fd-4167-bb9-iac-bucket"
        GOOGLE_APPLICATION_CREDENTIALS = "${WORKSPACE}/sa-key.json"
        TFVARS_FILE = "${params.ENVIRONMENT}.tfvars"
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
                sh """
                terraform init --backend-config="bucket=${env.GCS_BUCKET}" --backend-config="prefix=${params.ENVIRONMENT}"
                """
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
                sh "terraform plan -var-file=${env.TFVARS_FILE}"
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
                sh "terraform apply -var-file=${env.TFVARS_FILE} --auto-approve"
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
                sh "terraform destroy -var-file=${env.TFVARS_FILE} --auto-approve"
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