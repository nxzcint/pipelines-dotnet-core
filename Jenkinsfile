pipeline { 
  
  agent any
  
    stages { 
 
        stage('Build') {
            steps {
             bat "dotnet build --configuration Release"
            }
        }
 
        stage('Test'){ 
            steps { 
 
                sh "dotnet test" 
            } 
 
            post { 
                always { 
                    junit '**/target/surefire-reports/TEST-*.xml' 
                } 
            } 
        } 
 
        stage('Package'){ 
            steps { 
 
                sh "dotnet package" 
            } 
            post { 
                success { 
                    archiveArtifacts artifacts: '**/target/*.war', followSymlinks: false 
                } 
            } 
        } 
 
    } 
 
}
