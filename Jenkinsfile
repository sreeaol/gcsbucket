pipeline {
    agent any
    environment {
        stage('Prepare tfvars') {
    steps {
        sh '''
            cat > terraform.tfvars <<EOF
        PROJECT_ID    = "project-135912bf-7758-481f-965"
        region        = "us-central1"
        bucket_name   = "jenkins-gcs"
        storage_class = "STANDARD"
        EOF
        '''
    }
}
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                sh '''
                    terraform init
                '''
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
