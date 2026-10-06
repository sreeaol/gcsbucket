pipeline {
    agent any

    environment {
        PROJECT_ID = 'project-135912bf-7758-481f-965'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
stage('Debug tfvars') {
    steps {
        sh 'cat -n terraform.tfvars'
        sh 'od -c terraform.tfvars | head -20'
    }
}
        stage('Prepare tfvars') {
            steps {
                sh '''
                cat > terraform.tfvars <<EOF
                project_id    = "${PROJECT_ID}"
                region        = "US"
                bucket_name   = "jenkins-gcs"
                storage_class = "STANDARD"
                force_destroy = true
                EOF
                '''
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                input message: "Apply GCS bucket changes?"
                sh 'terraform apply -auto-approve tfplan'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/*.tfstate', fingerprint: true
        }
    }
}
