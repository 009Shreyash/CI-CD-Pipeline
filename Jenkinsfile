pipeline{
    
    agent any
    
    tools{
        maven "maven"
    }
    
    stages{
        stage("Git Clone"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/009Shreyash/CI-CD-Pipeline.git/']])   
            }
        }
        stage("Test"){
            steps{
                sh 'mvn test'
            }
        }
        stage("Build"){
            steps{
                sh 'mvn clean install'
            }
        }
         stage ('Install'){
            steps {
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }
       
        stage('DeployToTomcat'){
            steps{
                deploy adapters:[tomcat9(credentialsId: "TomcatCreds" , path: "", url:"http://192.168.0.75:8000/")], 
                contextPath: 'counterwebapp', 
                war: "target/*.jar"
            }
        }
        
    }
    
     post {
            success {
                junit checksName: 'Testss', testResults: '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'target/*.jar'
                }
             changed {
                script {
                    if (currentBuild.currentResult == 'FAILURE') { 
                        emailext subject: 'Jenkins Pipeline Failed',
                                 body: '$DEFAULT_CONTENT',
                        recipientProviders: [
                            [$class: 'CulpritsRecipientProvider'],
                            [$class: 'DevelopersRecipientProvider'],
                            [$class: 'RequesterRecipientProvider'] 
                        ], 
                        replyTo: '009shreyashraiyani@gmail.com',
                        to: '009shreyashraiyani@gmail.com'
                }
            }
        }
            }
       
}
