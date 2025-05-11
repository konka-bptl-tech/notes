# 09-05-2025
1. create jenkins server and agent server
step-1: create ec2 instance with ami amz3 instance t3a.small
step-2: create route53 record and access it from any ssh client
step-3: install java17 on jenkins machine from official amazon documents[https://docs.aws.amazon.com/corretto/latest/corretto-17-ug/amazon-linux-install.html]
step-4: Install Jenkins from official website access the jenkins http://<record>:<port>
step-5: Create a simple Hello World Job and Test it
2. Setup agent node
step-1: Launch another machine with t3a.small ami is amz linux
step-2: Create record for agent connect and install java17
3. Master Agent Setup
step-1: create credentials and username with password
step-2: copy the agent node private key and with username
step-3: create a new node in from jenkins
step-4: Create a first job and build in agent node by using passing label of agent node
4. Install Sonarqube and integrate with Jenkins
step-1: launch ubuntu instance t3a.medium
step-2: copy the script sonar.sh from daily used scripts run as root user that's it it will install sonarqube
step-3: access the sonarqube http://<record>:9000
step-4: generate the token and goto create credentials for sonarqube in jenkins take as secret text
step-5: add the plugin sonarqube-scanner
step-6: Manage Jenkins>System>SonarQube servers
5. Make jenkins agent as github runner also
step-1: inorder to configure githubactions runner we need .net installation so install .net
        wget -q https://dotnetcli.azureedge.net/dotnet/Sdk/9.0.100/dotnet-sdk-9.0.100-linux-x64.tar.gz -O dotnet-sdk.tar.gz
        mkdir -p $HOME/dotnet
        tar -xzf dotnet-sdk.tar.gz -C $HOME/dotnet/
        ln -sf $HOME/dotnet/dotnet /usr/local/bin/dotnet
        and install shashum sudo yum install perl-Digest-SHA
        sudo dnf install -y libicu
step-2: go to github > actions > runners copy and paste all commands
