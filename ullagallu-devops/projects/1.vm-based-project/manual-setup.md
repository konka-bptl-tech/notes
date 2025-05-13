# Stage-1[Manual Setup]
- Use default vpc
- All all traffic in default sg for resources
- Create RDS
  - Create R53 record for RDS endpoint
  - connect to rds using mysql
  - Create Schema on RDS
    - https://github.com/ullagallu123/spa-app/blob/main/mysql/tran.sql
- Create Elastic Cache
  - create subnet group
    add 2 subnets
  - Create R53 CNAME Record for redis endpoint  
- Launch EC2 instance using amazon ami
  - Create R53 record for backend
  - Connect to ec2 instance
    - install nodejs
      - curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
      - sudo dnf install nodejs
    - install telnet to test connectivity
      sudo yum install telnet
      telnet redis.konkas.tech 6379
    - Just install docker
      sudo yum install docker
      sudo service dockder start
      sudo docker run --rm -it --network host redis redis-cli \
        -h redis.konkas.tech \
        -p 6379 \
        --tls \
        --insecure
    - Clone the backend
      - mkdir /app
      - git clone https://github.com/sivaramakrishna-konka/vm-backend.git /app
      - sudo useradd -r -s /bin/false expense
      - sudo chown -R expense:expense /app
      - cd /app
      - npm install
      - vim /etc/systemd/system/backend.service
        [Unit]
        Description=Backend Service

        [Service]
        User=expense

        # Environment variables matching your Docker config
        Environment=DB_HOST="manual.konkas.tech"
        
        Environment=DB_USER="crud"

        Environment=DB_PASSWORD="CrudApp@1"

        Environment=DB_NAME="crud_app"

        Environment=REDIS_HOST="redis.konkas.tech"

        Environment=REDIS_PORT="6379"

        Environment=REDIS_TLS="true"

        Environment=REDIS_INSECURE="true

        # If using absolute paths for Node and your app

        ExecStart=/usr/bin/node /app/server.js


        SyslogIdentifier=backend

        StandardOutput=syslog

        StandardError=syslog

        [Install]

        WantedBy=multi-user.target
     
     - systemctl daemon-reload
     
     - systemctl start backend
     
     - systemctl enable backend
     
     - systemctl status backend
     
     - journalctl -u backend

- Today I'm trying setup RDS and Elastic Cache and backend
- Due to permissions issues POST is sucess where as fetch does not work due to lack of permissions
