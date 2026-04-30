`pip3 install fails because Python 3.11 doesn't always have all the wheels you expect on AL2023. Fix: pip3 install --break-system-packages -r requirements.txt, or use a venv.
run.sh works manually but not from user_data. Several students hit this in the transcript. The fix is to run gunicorn directly instead of going through run.sh. The reason is non-obvious — it's about how user_data shells handle backgrounded processes and PATH.
The app starts but crashes connecting to the DB. Almost always one of: wrong RDS endpoint, wrong password, RDS SG not allowing 5432 from ASG SG, or the env var not exported in the right scope.`

1. My User data worked but I have made some modification to
    the script to make it work. I spent a lot of time here.
    I ran each line individually to check where is the error and 
    tweak the script accordingly.
    Below is the script which I used:

#!/bin/bash
set -e
exec > /var/log/user-data.log 2>&1

# Wait for network to be ready
until curl -s https://google.com > /dev/null 2>&1; do
echo "Waiting for network..."
sleep 5
done

# Install dependencies
dnf install -y git python3-pip

# Clone app
cd /home/ec2-user
git clone https://github.com/akhileshmishrabiz/April26-bootcamp.git
cd April26-bootcamp/day2/app

# Install python dependencies
pip3 install -r requirements.txt

# Start the app with DB_LINK passed explicitly
nohup env DB_LINK="postgresql://postgres:postgres@bootcamp-db.cl44ysikkrs9.ap-south-1.rds.amazonaws.com:5432/mydb" \
gunicorn -b 0.0.0.0:8000 run:app > /home/ec2-user/app.log 2>&1 &

# Wait and verify
sleep 5
cat /home/ec2-user/app.log

echo "App started successfully"


