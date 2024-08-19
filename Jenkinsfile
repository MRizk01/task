pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/MRizk01/task.git', branch: 'master'
            }
        }
        stage('List Branches') {
            steps {
                sh 'git branch -a > branches'
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
                git log --oneline | grep "Hello" > hello_commit.txt
                '''
            }
        }
        stage('Update Black Branch with New Commit') {
            steps {
                sh '''
                git checkout black
                echo "hello again" >> hello_commit.txt
                echo "I need a cup of coffee" >> hello_commit.txt
                git add hello_commit.txt
                git commit -m "Update hello_commit.txt"
                git push origin black
                '''
            }
        }
        stage('Create Pull Request') {
            steps {
                sh '''
                git checkout master
                gh pr create --base master --head black --title "It’s Black" --body "Merging Black branch into master"
                '''
            }
        }
    }
}

