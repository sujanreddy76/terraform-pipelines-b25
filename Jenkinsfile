pipeline{
    agent {
        label 'terraform-slave'
    }
    stages {
        //initialize terraform
        stage('init') {
            steps {
                echo "Initializing the terraform"
            }

        }

        //plan
        stage('plan') {
            steps {
                echo "Executing the plan for terraform"
            }
        }

        //apply
        stage('apply') {
            steps {
                echo "Applying terraform infra"
            }
        }

        //destroy
        stage('destroy') {
            steps {
                echo "Destroying the terraform infra"
            }
        }
    }

}