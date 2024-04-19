pipeline {
    agent { 
                label 'linux'
            }
    tools {nodejs "nodejs"}
     parameters {
        choice choices: ['dev', 'staging', 'prod'], description: "Choose which environment to push changes to.", name: "DEPLOY_TO"
        choice(name: 'NODE_VERSION', choices: ['nodejs', 'nodejs6.9.1'], description: '')
        choice choices: ['US','UK','DE','ES','FR','IT','NL'], description: "Choose to which Country deploye website .", name: "DEPLOY_TO_COUNTRY" 
         //string(name: 'BUILD_NUMBER', description: 'Specify the build number to copy artifacts from')
        //booleanParam(name: 'skip_build', defaultValue: false, description: 'Set to true to skip the build stage')
         booleanParam(name: 'skip_compression', defaultValue: false, description: 'Set to true to skip the compression stage')
        // booleanParam defaultValue: true, "Choose whether to deploy the database.", name: "DEPLOY_DB"
    }
    environment {
        PM2_ECOSYSTEM_CONFIGS_US = "ecosystem.config.cjs"
        PM2_ECOSYSTEM_CONFIGS_UK = "ecosystem.config.js"
        PM2_ECOSYSTEM_CONFIGS_DE = "ecosystem.config.js"
        PM2_ECOSYSTEM_CONFIGS_ES = "ecosystem.config.js"
        PM2_ECOSYSTEM_CONFIGS_FR = "ecosystem.config.cjs"
        PM2_ECOSYSTEM_CONFIGS_IT = "ecosystem.config.cjs"
        PM2_ECOSYSTEM_CONFIGS_NL = "ecosystem.config.js"
        WEBSITE_PORTS_US = "3000"
        WEBSITE_PORTS_UK = "3001"
        WEBSITE_PORTS_DE = "3002"
        WEBSITE_PORTS_ES = "3003"
        WEBSITE_PORTS_FR = "3004"
        WEBSITE_PORTS_IT = "3005"
        WEBSITE_PORTS_NL = "3006"
        WORKSPACE = "${env.WORKSPACE}"
        TOKEN = credentials('telegram-token')
        CHAT_ID = credentials('telegram-chatid')
    //     branch = 'main'
    //     scmUrl = 'https://github.com/salmanspice/SimpleNodeJSAppForJenkins.git'
              }

    stages {
        stage("Pipeline Details"){
			steps{
				sh """
             echo "The current branch name  is ${env.BUILD_NUMBER}"
             echo "The current Build number  is ${env.BUILD_NUMBER}"
             echo "The current Job Name   is ${env.JOB_NAME}"
             echo "The current Workspace  is ${env.WORKSPACE}"
             echo "The current git branch  is ${env.GIT_BRANCH}"
             echo "The current git URL  is ${env.GIT_URL}"
	     """ 
        
			}
		}
        // stage("Checkout") {
        //     steps {
        //             checkout([$class: 'GitSCM',
        //                 branches: [
        //                     [name: '**']
        //                 ],
        //                 doGenerateSubmoduleConfigurations: false,
        //                 extensions: [[$class: 'LocalBranch', localBranch: "**"]],
        //                 submoduleCfg: [],
        //                 userRemoteConfigs: [
        //                     [credentialsId: 'new-git-qwik-cred', url: 'https://printerpix@dev.azure.com/printerpix/Engineering%20Frontend/_git/printerpixqwikui/']
        //                 ]
        //             ])
        //         }
        //     }

            // stage("Branch To Build") {
            // steps {
            //         script {
            //             def gitBranches = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref --all | sed s:origin/:: | sort -u')
            //             env.BRANCH_TO_BUILD = input message: 'Please select a branch', ok: 'Continue',
            //                 parameters: [choice(name: 'BRANCH_TO_BUILD', choices: gitBranches, description: 'Select the branch to build?')]
            //         }
            //         git branch: "${env.BRANCH_TO_BUILD}", credentialsId: 'new-git-qwik-cred', url: 'https://printerpix@dev.azure.com/printerpix/Engineering%20Frontend/_git/printerpixqwikui/'
            //     }
            // }
            
	// 	stage('SonarQube analysis') {
    //     environment {
    //         scannerHome = tool 'sonar-scanner-printerpix'
    //     }
    //     steps {
    //         withSonarQubeEnv('sonar-jenkins') {
    //             sh '''
    //             ${scannerHome}/bin/sonar-scanner \
    //             -D sonar.projectKey=qwik-jenkins \
    //             -D sonar.projectName=qwik-jenkins \
    //             -D sonar.sources=.
    //             '''
    //         }
    //     }
    // }
        //   stage('build') {
           
        //       agent { 
        //         label 'linux'
        //     }
        //      when { expression { params.skip_build != true } }
        //       steps {
        //         echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
        //           echo 'building the nodejs application'
        //           //sh 'npm install'
        //           //sh 'npm install && npm i && npm run build'
        //           sh 'which node'
        //           sh 'npm -v'
        //          // sh  'node -v > /var/www/www.printerpix.com/.node-version'
        //           //sh 'npm install && npm install qwik && npm install sharp && npm install preflight-check  && npm i && npm run build'
        //           sh 'npm install && npm run build'  
        //       }
        //   }

    // stage('Compress Build Artifacts'){
    //          agent { 
    //             label 'linux'
    //         }
    //          when { expression { params.skip_compression != true } }
    //         steps {
    //            echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
    //              sh """
    //             echo "BUILD_NUMBER" :: $BUILD_NUMBER
    //             #echo "JOB_NAME" :: /var/lib/jenkins/workspace/${env.JOB_NAME}
    //            cd /var/lib/jenkins/workspace/${env.JOB_NAME}@2

    //             tar -zcvf node_modules.tar.gz node_modules
    //             tar -zcvf dist.tar.gz dist
    //             tar -zcvf server.tar.gz server
    //              """
    //           }
    //        }

    //  stage('Archieving Build  Artifact')
    //   {
    //        agent { 
    //       label 'linux'
    //   }
    //      steps{
    //              archiveArtifacts artifacts: 'node_modules.tar.gz,ecosystem.config.cjs,dist.tar.gz,server.tar.gz,package.json', followSymlinks: false
    //            }
    //   }

    stage('Copy Artifact') {
      steps {
            copyArtifacts (
                filter: "node_modules.tar.gz,ecosystem.config.cjs,dist.tar.gz,server.tar.gz,package.json", 
                //filter: "**", 
                fingerprintArtifacts: true, 
                projectName: "Dev-Qwik-Website-Deployment", 
                selector: buildParameter('ROLLBACK_BUILD')
            )
        }
    }

          stage('Send Build Artifcats & Deploy US website on  dev Server-10.10.28.21'){
             agent { 
                label 'linux'
            }
           
            // when {
            //     branch 'dev'
            // }
            when {
               //expression  {
                      environment ignoreCase: true, name: "DEPLOY_TO", value: "dev"
                      environment ignoreCase: true, name: "DEPLOY_TO_COUNTRY", value: "US"
                      
            //}
            }
            steps {
                echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
                
                //sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: ' ', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server/**,ecosystem.config.cjs,package.json')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
                //sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /var/www/www.printerpix.com && tar -xvzf dist.tar.gz && tar -xvzf node_modules.tar.gz && tar -xvzf server.tar.gz && pm2 restart /var/www/www.printerpix.com/ecosystem.config.cjs', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'node_modules.tar.gz,ecosystem.config.cjs,dist.tar.gz,server.tar.gz,package.json')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
               //sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /var/www/www.printerpix.com && tar -xvzf dist.tar.gz && tar -xvzf node_modules.tar.gz && tar -xvzf server.tar.gz && pm2 restart /var/www/www.printerpix.com/ecosystem.config.cjs', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'node_modules.tar.gz,ecosystem.config.cjs,dist.tar.gz,server.tar.gz,package.json')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
               sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])


            }
        post {
        success {
           script {
                sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} succeeded!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
            }
         }
        failure {
             script {
                 sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} failed!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
             }
         }
          }
        } 
      
          stage('Send Build Artifcats & Deploy FR website on  dev Server-10.10.28.21'){
             agent { 
                label 'linux'
            }
           
            // when {
            //     branch 'dev'
            // }
            when {
               //expression  {
                     environment ignoreCase: true, name: "DEPLOY_TO", value: "dev"
                      environment ignoreCase: true, name: "DEPLOY_TO_COUNTRY", value: "FR"
                      
            //}
            }
            steps {
                echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
                
                //sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: ' ', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server/**,ecosystem.config.cjs,package.json')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
                sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-france', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /var/www/www.printerpix.fr && tar -xvzf dist.tar.gz && tar -xvzf node_modules.tar.gz && tar -xvzf server.tar.gz && pm2 restart /var/www/www.printerpix.fr/ecosystem.config.cjs', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server.tar.gz,dist.tar.gz,node_modules.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }
        post {
        success {
           script {
                sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} succeeded!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
            }
         }
        failure {
             script {
                 sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} failed!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
             }
         }
          }
        }
        stage('Send Build Artifcats & Deploy UK website on  dev Server-10.10.28.21'){
             agent { 
                label 'linux'
            }
           
            // when {
            //     branch 'dev'
            // }
            when {
               //expression  {
                      environment ignoreCase: true, name: "DEPLOY_TO", value: "dev"
                      environment ignoreCase: true, name: "DEPLOY_TO_COUNTRY", value: "UK"
                      
            //}
            }
            steps {
                echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
                //sed -i -E "s/(.*PORT.*:).*/\1 \"$WEBSITE_PORT_UK\",/g" "$WEBSITE_PM2_CONFIG_FILE_UK"
                
                //sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: ' ', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server/**,ecosystem.config.cjs,package.json')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
                sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-uk', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /var/www/www.printerpix.co.uk && tar -xvzf dist.tar.gz && tar -xvzf node_modules.tar.gz && tar -xvzf server.tar.gz &&  pm2 start /var/www/www.printerpix.co.uk/ecosystem.config.js', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server.tar.gz,dist.tar.gz,node_modules.tar.gz,ecosystem.config.cjs')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }
        post {
        success {
           script {
                sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} succeeded!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
            }
         }
        failure {
             script {
                 sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} failed!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
             }
         }
          }
        }

        stage('Send Build Artifcats & Deploy DE website on  dev Server-10.10.28.21'){
             agent { 
                label 'linux'
            }
           
            // when {
            //     branch 'dev'
            // }
            when {
               //expression  {
                     environment ignoreCase: true, name: "DEPLOY_TO", value: "dev"
                      environment ignoreCase: true, name: "DEPLOY_TO_COUNTRY", value: "DE"
                      
           // }
            }
            steps {
                echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL}"
                
                //sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-dev', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: ' ', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server/**,ecosystem.config.cjs,package.json')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
                sshPublisher(publishers: [sshPublisherDesc(configName: 'qwik-de', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'cd /var/www/www.printerpix.de && tar -xvzf dist.tar.gz && tar -xvzf node_modules.tar.gz && tar -xvzf server.tar.gz &&   pm2 start /var/www/www.printerpix.de/ecosystem.config.js', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'server.tar.gz,dist.tar.gz,node_modules.tar.gz')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }
        post {
        success {
           script {
                sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} succeeded!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
            }
         }
        failure {
             script {
                 sh "curl -X POST -H \"Content-Type: application/json\" -d \"{\\\"chat_id\\\":${CHAT_ID}, \\\"text\\\": \\\"build ${env.BUILD_ID} ,build name  ${env.JOB_NAME} failed!\\\", \\\"disable_notification\\\": false}\" https://api.telegram.org/bot${TOKEN}/sendMessage"
             }
         }
          }
        }

         
    


 }
}
