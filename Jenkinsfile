pipeline {
    agent any
    
    stages {
        stage("checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/nesrinetry/course3-jenkins-gs-spring-petclinic.git'
            }
        }
        
        stage("Build") {
            steps {
                script {
                    // Use the 'script' block to run arbitrary Groovy code
                    env.JAVA_HOME = 'C://Program Files//Java//jdk-17'
                    bat "./mvnw package"
                }
            }
        }
        
        stage("capture") {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
                jacoco()
                junit '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
    
    post {
        regression {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
            to: 'always@foo.bar',
            recipientProviders: [previous()],
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}
