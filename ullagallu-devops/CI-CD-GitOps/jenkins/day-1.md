# Jenkins Installation
1. Jenkins is a java based web application so to run jenkins app we java runtime we were using amz3
```bash
sudo yum install java-17-amazon-corretto-headless
```
2. Install Jenkins from offcial website
```bash
https://www.jenkins.io/doc/book/installing/linux/
```
3. Jenkins is comes with distributed architecture where we have Master for Jobs distribution to the workers can execute the jobs
4. To execute the jobs in worker we need to install java on worker nodes repeat point 1

