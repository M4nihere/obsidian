
# Code Deploy
## Create a service role (console)
root@prod-backend:/etc/systemd/system# cat celery-beat.service
[Unit]
Description=Celery Beat Scheduler
After=network.target

[Service]
Environment="CELERY_BROKER_URL=redis://:9r3m8FO1qy6as9BfaHX@13.210.189.183:6379/0"
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/groovio
ExecStart=/home/ubuntu/groovio/venv/bin/celery -A groovio beat --loglevel=info
ExecStop=/home/ubuntu/groovio/venv/bin/celery -A groovio control shutdown
Restart=always
TimeoutSec=60

[Install]
WantedBy=multi-user.target