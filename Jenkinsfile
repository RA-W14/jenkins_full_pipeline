pipeline {
    agent any

    environment {
        // PROD_SSH = "user@your-prod-server-ip-or-hostname"
        // PROD_PATH = "/home/user/twitter_for_pets"
        // SSH_KEY_ID = "prod-ssh-key"
        PATH = "C:/Users/Toh Hong Yun/AppData/Local/Programs/Python/Python311;C:/Users/Toh Hong Yun/AppData/Local/Programs/Python/Python311/Scripts/;${env.PATH}"
    }

    stages {
        stage('Lint') {
            steps {
                bat '''
                pip install -r req
                pip install ruff mypy
                ruff check .
                mypy twitter_for_pets.py
                '''
            }
        }

        stage('Test') {
            steps {
                bat '''
                python -m unittest test_twitter_for_pets
                '''
            }
        }

        stage('Deploy') {
            when {
                tag "release-*"
            }
            steps {
                sshagent (credentials: [SSH_KEY_ID]) {
                    // Use Git Bash to run Linux SSH commands from Windows Jenkins
                    bat '''
                    bash -c "ssh $PROD_SSH 'pkill -f twitter_for_pets.py || true; mkdir -p $PROD_PATH'"
                    bash -c "scp twitter_for_pets.py requirements.txt $PROD_SSH:$PROD_PATH/"
                    bash -c "ssh $PROD_SSH '
                        cd $PROD_PATH
                        if [ ! -d venv ]; then
                            python3 -m venv venv
                        fi
                        source venv/bin/activate
                        pip install -r requirements.txt
                        nohup python3 twitter_for_pets.py > server.log 2>&1 &
                    '"
                    '''
                }
            }
        }
    }
}

