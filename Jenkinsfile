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
    stages {
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

}