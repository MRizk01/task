pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/MRizk01/task.git'
            }
        }
        stage('List Branches') {
            steps {
                sh 'git branch -a'
            }
        }
        stage('Make File Executable') {
            steps {
                sh 'chmod a+x branches'
            }
        }
        stage('Get Hello Commit from Red Branch') {
            steps {
                sh '''
                git checkout red
                git log --oneline | grep Hello > hello_commit.txt
                '''
            }
        }
        stage('Update Black Branch with New Commit') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'fb4df0b3-3a24-4e12-b44a-a5e5b2633025', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh '''
                    git checkout black
                    git config user.name "MRizk01"
                    git config user.email "mrizkrageh@gmail.com"
                    echo hello again >> hello_commit.txt
                    echo I need a cup of coffee >> hello_commit.txt
                    git add hello_commit.txt
                    git commit -m "Update hello_commit.txt"
                    git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/MRizk01/task.git black
                    '''
                }
            }
        }
        stage('Create Pull Request') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'fb4df0b3-3a24-4e12-b44a-a5e5b2633025', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh '''
                    gh auth login --with-token <<< $GIT_PASSWORD
                    gh pr create --base master --head black --title "It’s Black" --body "Merging Black branch into master"
                    '''
                }
            }
        }
    }
}
