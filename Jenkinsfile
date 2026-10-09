pipeline {
    agent any

    parameters {
        string   (
        name  : 'BUCKET_NAME'
        defaultvalue:''
        
        
    }

    stages {
        stage('validate input') {
            steps {
                script { 
                    if(!params.bucketname?.trim() { 
                        error ('bucketname must be provided')
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
