pipeline {
    agent any

    parameters {
        string(name: 'BUCKET_NAME', defaultValue: 'default-bucket-name', description: 'Name of the GCS bucket')
    }

    environment {
        PROJECT_ID    = "project-135912bf-7758-481f-965"
        REGION        = "us-central1"
        STORAGE_CLASS = "STANDARD"
    }

  stage('Prepare tfvars') {
    steps {
        sh """
        cat > terraform.tfvars <<'EOF'
        project_id    = "${PROJECT_ID}"
        region        = "${REGION}"
        bucket_name   = "${BUCKET_NAME}"
        storage_class = "${STORAGE_CLASS}"
        EOF
        """
    }
}

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -var-file=terraform.tfvars'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve -var-file=terraform.tfvars'
            }
        }
    }
}
