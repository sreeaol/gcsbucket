pipeline {
    agent any

    environment {
        PROJECT_ID    = "project-135912bf-7758-481f-965"
        REGION        = "us-central1"
        BUCKET_NAME   = "medha-sreejith"
        STORAGE_CLASS = "STANDARD"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Prepare tfvars') {
            steps {
                sh '''
                    cat > terraform.tfvars <<EOF
project_id    = "${PROJECT_ID}"
region        = "${REGION}"
bucket_name   = "${BUCKET_NAME}"
storage_class = "${STORAGE_CLASS}"
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
                sh '''
                    terraform plan \
                      -var="project_id=${PROJECT_ID}" \
                      -var="region=${REGION}" \
                      -var="bucket_name=${BUCKET_NAME}" \
                      -var="storage_class=${STORAGE_CLASS}"
                '''
            }
        }

        stage('Terraform Apply') {
            steps {
                sh '''
                    terraform apply -auto-approve \
                      -var="project_id=${PROJECT_ID}" \
                      -var="region=${REGION}" \
                      -var="bucket_name=${BUCKET_NAME}" \
                      -var="storage_class=${STORAGE_CLASS}"
                '''
            }
        }
    }
}
