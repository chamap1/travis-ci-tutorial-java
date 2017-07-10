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
        RELEASE_TAG_NAME = 'vtesting'
        RELEASE_NAME = 'testing'
        RELEASE_BODY = 'testing'
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
                doCompile()
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
        stage('Github Release'){
            steps{
                sh "echo hi thi sis ${RELEASE_TAG_NAME}"
                githubReleaseTest()
            }
        }
    }
    post{
        always{
            cleanWorkspace() 
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
