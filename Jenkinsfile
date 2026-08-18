pipeline{
    agent any
    tools{
        maven "mymaven"
    }
    stages{
        stage("code"){
            steps{
                git "https://github.com/bharathperala/java-application-using-eks.git"
            }
        }
       stage("cqa"){
           steps{
                withSonarQubeEnv("mysonar") {
                 sh """
                        mvn clean verify \
                        org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar \
                        -Dsonar.projectKey=cqa
                    """
              }
           }
       }
       stage("build"){
           steps{
               sh "mvn clean install"
           }
       }
       stage("image"){
           steps{
               withDockerRegistry(credentialsId: 'f04d5f41-b934-4cee-8906-35086ae42b25') {
                sh "docker pull bharath0803/project:dbimage-v1"
                sh "docker pull bharath0803/project:appimage-v1"
             }
           }
       }
    }
}
