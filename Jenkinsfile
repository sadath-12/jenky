// pipeline {
    
// 	agent any
	
// 	tools {
//         maven "MAVEN3"
//         jdk "OracleJDK8"
//     }

//     environment {
//         SNAP_REPO = 'vprofile-snapshot'
//         NEXUS_USER = 'admin'
//         NEXUS_PASS = 'admin123'
//         RELEASE_REPO = 'vprofile-release'
//         CENTRAL_REPO = 'vipro-maven-central'
//         NEXUSIP = '172.31.36.25'
//        NEXUSPORT = '8081'
//        NEXUS_GRP_REPO = 'vipro-maven-group'
//         NEXUS_LOGIN = "nexuslogin"
//         SONARSERVER = 'sonarserver'
//         SONARSCANNER = 'sonarscanner'
//     }
	
//     stages{
        
//         stage('BUILD'){
//             steps {
//                 sh 'mvn -s settings.xml -DskipTests install'
//                     // sh 'mvn clean install -DskipTests'
//             }
//             post {
//                 success {
//                     echo 'Now Archiving...'
//                     archiveArtifacts artifacts: '**/target/*.war'
//                 }
//             }
//         }

// 	stage('UNIT TEST'){
//             steps {
//                 sh 'mvn -s settings.xml test'
//                 //   sh 'mvn test'
//             }
//         }

// 	// stage('INTEGRATION TEST'){
//     //         steps {
//     //             sh 'mvn -s settings.xml verify -DskipUnitTests'
//     //             //   sh 'mvn verify -DskipUnitTests'
//     //         }
//     //     }
		
//         stage ('CODE ANALYSIS WITH CHECKSTYLE'){
//             steps {
//                 sh 'mvn -s settings.xml checkstyle:checkstyle'
//                     // sh 'mvn checkstyle:checkstyle'
//             }
//             post {
//                 success {
//                     echo 'Generated Analysis Result'
//                 }
//             }
//         }

//         stage('CODE ANALYSIS with SONARQUBE') {
          
// 		  environment {
//              scannerHome = tool "${SONARSCANNER}"
//           }

//           steps {
//             withSonarQubeEnv("${SONARSERVER}") {
//                sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
//                    -Dsonar.projectName=vprofile-repo \
//                    -Dsonar.projectVersion=1.0 \
//                    -Dsonar.sources=src/ \
//                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
//                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
//                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
//                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
//             }

//             timeout(time: 10, unit: 'MINUTES') {
//                waitForQualityGate abortPipeline: true
//             }
//           }
//         }

//         stage("Publish to Nexus Repository Manager") {
//             steps {
//             nexusArtifactUploader(
//                 nexusVersion: 'nexus3',
//                 protocol: 'http',
//                 nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
//                 groupId: "QA",
//                 version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
//                 repository: "${RELEASE_REPO}",
//                 credentialsId: "${NEXUS_LOGIN}",
//                 artifacts:[
//                     [artifactId:'vproapp',
//                     classifier:'',
//                     file:'target/vprofile-v2.war',
//                     type:'war'
//                     ]
//                 ]
//             )
//             }
//         }

//         stage('Ansible Deploy to staging'){
//             steps{
//                 ansiblePlaybook([
//                     inventory: 'ansible/stage.inventory',
//                     playbook : 'ansible/site.yml',
//                     installation: 'ansible',
//                     colorized: true ,
//                     credentialsId: 'applogin',
//                     disableHostKeyChecking: true ,
//                     extraVars: [
//                         USER: 'admin',
//                         PASS: 'admin123',
//                         nexusip: '172.31.36.25',
//                         reponame: "vprofile-release",
//                         groupId: "QA",
//                         time: "${env.BUILD_TIMESTAMP}",
//                         build: "${env.BUILD_ID}",
//                         artifactid: "vproapp",
//                         vprofile_version: "vproapp-${env.BUILD_ID}-${env.BUILD_TIMESTAMP}.war"
//                     ]
//                 ])
//             }
//         }


//     }


// }


// for production


pipeline {
    
	agent any
	
	tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vipro-maven-central'
        NEXUSIP = '172.31.36.25'
       NEXUSPORT = '8081'
       NEXUS_GRP_REPO = 'vipro-maven-group'
        NEXUS_LOGIN = "nexuslogin"
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
    }
	
    stages{
        
        stage('Setup params'){
            steps{
                script{
                    properties([
                        parameters([
                            string(
                                defaultValue:'',
                                name: 'BUILD'
                            ),
                            string(
                                defaultValue:'',
                                name: 'TIME'
                            )
                        ])
                    ])
                }
            }
        }

        stage('Ansible Deploy to staging'){
            steps{
                ansiblePlaybook([
                    inventory: 'ansible/stage.inventory',
                    playbook : 'ansible/site.yml',
                    installation: 'ansible',
                    colorized: true ,
                    credentialsId: 'applogin',
                    disableHostKeyChecking: true ,
                    extraVars: [
                        USER: 'admin',
                        PASS: 'admin123',
                        nexusip: '172.31.36.25',
                        reponame: "vprofile-release",
                        groupId: "QA",
                        time: "${env.BUILD_TIMESTAMP}",
                        build: "${env.BUILD_ID}",
                        artifactid: "vproapp",
                        vprofile_version: "vproapp-${env.BUILD_ID}-${env.BUILD_TIMESTAMP}.war"
                    ]
                ])
            }
        }


    }


}


// token = bc65a8fe0311ed3757a0b9bb6b5bb55a86690f67
// upload singhkey as app 

// install ansible in jenkins 
// sudo apt-get update
// sudo apt-get install software-properties-common
// sudo apt-add-repository ppa:ansible/ansible
// sudo apt-get update
// sudo apt-get install ansible
// ansible --version


// install ansible plugin 