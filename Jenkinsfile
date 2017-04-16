//@Library('shared-libraries') _
pipeline {
    agent {
        node{
//          label 'maven-builder'
            label 'builder-06'
            customWorkspace "workspace/${env.JOB_NAME}"
            }
    }
    parameters {
        string(name: 'REPO_NAME', 
               defaultValue: '${JOB_NAME}', 
               description: 'Name of the dellemc-symphony repository')
    }
    environment {
        //GIT_CREDS = credentials('github-04')
        GITHUB_TOKEN = credentials('github-04')
    }
    tools {
        maven 'linux-maven-3.3.9'
        jdk 'linux-jdk1.8.0_102'
    }
    options { 
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
    }
    stages {
        stage('Compile') {
            steps {
//                script {
//                    def stages = new com.dell.cpsd.SCM.Stages()
//                    stages.doCompile()     
//                }
                echo "${params.REPO_NAME}"
                echo "${env.JOB_NAME}.replaceAll('git-org-test-purna/','').replaceAll('/${BRANCH_NAME}','')"
                doCompile()
//                build  job:'gitorg-test-purna/simple-build-for-pipeline-plugin/master', wait: false
//                sh "mvn compile"
            }
        }
        stage('Deploy') {
            steps {
                sh "mvn clean install"
                sh "echo This is not deploying to any remote repository. Just for testing purpose."
            }
        }
        stage('Unit Tests') {
            steps {
                sh "mvn test"
            }
        }
        stage('Integration Tests') {
            steps {
                sh "echo No Integration tests defined for this repo!"
            }
        }
        stage('SonarQube Analysis') {
            steps{
                doSonarAnalysis()
            }
        }
        stage('Third Party Audit'){
            steps{
                doThirdPartyAudit()
            }
        }
        stage('NexB Scan'){
            steps{
                doNexbScanning()
            }
        }
        stage('Github Release'){
            steps{
                githubRelease()
            }
        }
    }
    post{
        always{
            step([$class: 'WsCleanup'])   
        }
        success{
            //steps to be performed
            sh "echo Do Nothing"
        }
        failure{
            //steps to be performed
            sh "echo Do Nothing"
        }
        unstable{
            //steps to be performed
            sh "echo Do Nothing"
        }
    }
}
