pipeline{
    agent any
    tools{
    jdk 'jdk-21'
    maven 'maven-3.9'
    }
    stages{
        stage('saludo'){
            steps{
                echo 'Hola desde Jenkins'
            }
        }
        stage('java'){
            steps{
                bat 'java -version'
            }
        }
        stage('compilar'){
            steps{
                bat 'mvn -B clean compile'
            }
        }
        stage('probar') {
            steps {
                bat 'mvn -B test'
            }
            post{
                always {
                    junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
                }
            }
        }
        stage('empaquetar') {
            steps {
                bat 'mvn -B package -DskipTests'

                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

    }

}