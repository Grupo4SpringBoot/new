pipeline{
    agent any
    tools{
        jdk 'jdk-21'
        maven 'maven-3.9'
    }
    parameters{
        booleanParam(name: 'EJECUTAR_TESTS', defaultValue: true, description: '¿Ejecutar pruebas?')

        booleanParam(name: 'EJECUTAR_PRUEBAS', defaultValue: true, description: 'Marcar para ejecutar las pruebas. Desmarcar para saltarla.')

        booleanParam(name: 'EMPAQUETAR', defaultValue: true, description: 'Marcar para generar y guardar el JAR. Desmarcar para omitir')
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
            when{
                expression{return  params.EMPAQUETAR}
            }
            steps{
                bat 'mvn -B package -DskipTests'
                archiveArtifacts: 'target/*.jar'
            }
        }
        stage('pruebas'){
            when{
                expression { params.EJECUTAR_TESTS == true}
            }
            steps{
                echo 'Ejecutando pruebas..'
            }
        }
        stage('probar'){
            steps{
                bat "mvn -B test -DsipTests= ${!params.EJECUTAR_PRUEBAS}"
            }
            post{
                always{
                    junit testResults: 'target/surefire-reports/*.xml, allowEmptyResults: true'
                }
            }
        }
    }
    
    post{
        failure{
            echo "Fallo: la construccion ${env.BUILD_NUMBER} de ${env.JOB_NAME} ha fallado. Revisar la etapa en rojo."
        }

        success {
            echo "Ok: la construccion ${env.BUILD_NUMBER} de ${env.JOB_NAME} ha terminado bien."
        }
        fixed{
            echo "Recuperada: ${env.JOB_NAME} vuelve a funcionar desde la construccion ${env.BUILD_NUMBER}"
        }
    }

}