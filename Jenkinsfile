pipeline {

    agent {
        
        label "master"
    }
    tools{
        maven "maven"
    }
    triggers{ cron('H H(9-16)/2 * * 1-5') }
    options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '10'))
    }  
    stages {
        
        stage("Maven Build") {
            
            steps {

                script {
                sh "mvn package -DskipTests=true"
                }
                }

            }
       stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv('sonar') {
                sh 'mvn sonar:sonar'
                  }
             }
           } 
        
        stage('Build Docker Image') {
            steps {
                
                script {
      sh "$docker_path/docker build -t np:${BUILD_NUMBER} ."
    
            }
        }
        }
        stage("archiveArtifacts") {
            steps {
                   archiveArtifacts artifacts: 'target/np.jar', followSymlinks: false
            }
        }
         stage("clean Workspace") {
            steps {
                    cleanWs()
            }
         }
        
    }
}
