pipeline{
    agent any
    
    environment{
        SONAR_HOME=tool "sonar"
    }
    
    
    stages{
        stage("Code clone from github"){
            steps{
                git url: "https://github.com/Hulk322/wanderlust-multistage.git", branch:"main"
            }
        }
        stage("SonarQube quality analysis"){
            steps{
                withSonarQubeEnv("sonar"){
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
        stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Sonar Quality Gate Scan"){
            steps{
                timeout(time: 2, unit: "MINUTES"){
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File system scan"){
            steps{
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("build"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
