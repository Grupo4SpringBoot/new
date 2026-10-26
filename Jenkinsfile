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
    post{
        failure{
            echo "Fallo: la construccion ${env.BUILD_NUMBER} de ${env.JOB_NAME} ha fallado. Revisar la etapaenrojo."
        }

        success {
            echo "Ok: la construccion ${env.BUILD_NUMBER} de ${env.JOB_NAME} ha terminado bien."
        }
        fixed{
            echo "Recuperada: ${env.JOB_NAME} vuelve a funcionar desde la construccion ${env.BUILD_NUMBER}"
        }
    }

}