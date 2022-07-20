pipeline {
    agent any

    environment {

        AWS_ACCESS_KEY_ID     = credentials('aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')

        AWS_S3_BUCKET = "dotnet-bucket-ahmed"
        ARTIFACT_NAME = "hello-world.dll"
        AWS_EB_APP_NAME = "dotnet-application"
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        AWS_EB_ENVIRONMENT = "Dotnetapplication-env"

        SONAR_IP = "54.226.50.200"
        SONAR_TOKEN = "sqp_aa3cba40e3342d9cff9044e498766a66cf8cc0cc"

    }

    stages {
        stage('Validate') {
            steps {
                
                sh "dotnet restore"

                

            }
        }

         stage('Build') {
            steps {
                
                sh "dotnet build"

            }
        }

        stage('Test') {
            steps {
                
                sh "dotnet test"

            }

            
        }

       

        stage('Package') {
            steps {
                
                sh "dotnet publish"

            }

            post {
                success {
                    archiveArtifacts artifacts: '**/bin/Debug/net6.0/**.dll', followSymlinks: false

                   
                }
            }
        }

        stage('Publish artefacts to S3 Bucket') {
            steps {

                sh "aws configure set region us-east-1"

                sh "aws s3 cp ./bin/Debug/net6.0/**.dll s3://$AWS_S3_BUCKET/$ARTIFACT_NAME"
                
            }
        }

        stage('Deploy') {
            steps {

                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'

                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
            
                
            }
        }
        
    }
}
