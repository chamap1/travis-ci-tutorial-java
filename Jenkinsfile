//@Library('shared-libraries') _
pipeline {
    agent {
        node{
//          label 'maven-builder'
            label 'builder-06'
            customWorkspace "workspace/${env.JOB_NAME}"
            }
    }
    environment {
        GITHUB_TOKEN = credentials('github-04')
    }
    tools {
        maven 'linux-maven-3.3.9'
        jdk 'linux-jdk1.8.0_102'
        jdk 'linux-jdk1.7.0_79'
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
                sh "echo do nothing"
//               doNexbScanning()
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
