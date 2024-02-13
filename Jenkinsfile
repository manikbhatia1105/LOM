pipeline {

   agent {
         label 'manipal-ecomm-bed-dev'
         }

   stages {

     stage ('Docker Deployment') {
       steps {
          echo "copying the latest file in Docker container"
          sh 'sudo docker cp . manipal-ecomm-dev-BED:/usr/src/app'
             }
         }

     stage  ('python packages installation') {
       steps {
          echo "installing the packages from requirement.txt file"
          sh 'sudo docker exec manipal-ecomm-dev-BED  pip install -r requirements.txt'
             }
         }

     stage  ('static files') {
       steps {
          echo "running collectstatic command"
          sh 'sudo docker exec manipal-ecomm-dev-BED python manage.py collectstatic --noinput'
             }
         }


  stage  ('Cron Remove') {
       steps {
          echo "removing crons in crontab"
          sh 'sudo docker exec manipal-ecomm-dev-BED python manage.py crontab remove'
             }
         }
     stage  ('Cron addition') {
       steps {
          echo "adding crons in crontab"
          sh 'sudo docker exec manipal-ecomm-dev-BED python manage.py crontab add'
             }
         }

     stage ('Restart Docker') {
       steps {
          echo "restarting the docker"
          sh "sudo docker restart manipal-ecomm-dev-BED"
          sh "sudo docker exec  manipal-ecomm-dev-BED bash /etc/init.d/cron start"

             }
         }
     }
}
