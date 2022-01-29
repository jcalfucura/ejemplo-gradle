import groovy.json.JsonSlurperClassic

def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    stages {
        stage("Paso 0: Download and checkout"){
            steps {
                checkout(
                    [$class: 'GitSCM',
                    //Acá reemplazar por el nonbre de branch
                    branches: [[name: "feature-nexus" ]],
                    //Acá reemplazar por su propio repositorio
                    userRemoteConfigs: [[url: 'https://github.com/jcalfucura/ejemplo-maven.git']]])
            }
        }
        stage("Paso 1: Compliar"){
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean compile -e"
                }
            }
        }
        stage("Paso 2: Testear"){
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean test -e"
                }
            }
        }
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "mvn clean package -e"
                }
            }
        }
        stage("Paso 4: Análisis SonarQube"){
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "echo 'Calling sonar Service in another docker container!'"
                    // Run Maven on a Unix agent to execute Sonar.
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }
        stage("Paso 5: Subida a Nexus") {
            //record the test results and archive the jar file.
            steps {
                //archiveArtifacts artifacts:'build/*.jar'
                nexusPublisher nexusInstanceId: 'nexus',
                    nexusRepositoryId: 'devops-usach-nexus',
                    packages: [
                            [$class: 'MavenPackage',
                                mavenAssetList: [
                                    [classifier: '',
                                    extension: '',
                                    filePath: 'build/DevOpsUsach2020-0.0.1.jar']
                                ],
                        mavenCoordinate: [
                            artifactId: 'DevOpsUsach2020',
                            groupId: 'com.devopsusach2020',
                            packaging: 'jar',
                            version: '0.0.1']
                        ]
                    ]
                }
        }

        stage('Paso 6: Bajar Nexus Stage') {
            steps {
                sh 'curl -X GET -u admin:calfu1975 http://nexus:8081/repository/devops-usach-nexus/com/devopsusach2020/DevOpsUsach2020/0.0.1/DevOpsUsach2020-0.0.1.jar -O'
            }
        }                

        stage("Paso 7: Levantar Springboot APP"){
            steps {
                // sh 'mvn spring-boot:run &'
                sh 'java -jar DevOpsUsach2020-0.0.1.jar &'
            }
        }
        stage("Paso 8: Dormir(Esperar 10sg) "){
            steps {
                sh 'sleep 30'
            }
        }

        stage("Paso 9: Test Alive Service - Testing Application!"){
            steps {
                sh 'curl -X GET "http://localhost:8080/rest/mscovid/test?msg=testing"'
            }
        }
    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
        success {
            sh "echo 'fase success'"
        }

        failure {
            sh "echo 'fase failure'"
        }
    }
}