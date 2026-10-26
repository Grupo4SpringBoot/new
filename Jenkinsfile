pipeline{
    agent any
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
    }
}