pipeline { 
  
  agent any
  
    stages { 
  
      stage('restore') {
            steps {
             sh "dotnet restore"
            }
        }
      
      
        stage('Build') {
            steps {
             sh "dotnet build"
            }
        }
 
        stage('Test'){ 
            steps { 
 
                sh "dotnet test" 
            } 
 
        } 
 
        stage('Package'){ 
            steps { 
 
                sh "dotnet publish" 
            } 
            post { 
                success { 
                    archiveArtifacts artifacts: '**/bin/Debug/net6.0/**.dll', followSymlinks: false 
                } 
            } 
        } 
 
    } 
 
}
