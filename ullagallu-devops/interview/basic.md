# Tell me about yourself

# Why do you switch the job

# Why do you have career gap

# What's your day to day activities

# What's your Roles and Responsibilites

# How do you configure devops to a your project
- setup
- operations[run the setup without any problems]
- improvements

For devops engineers developers are clients and customers we let them only to do best coding apart from that we will take care of everything dev to prod

Developers:
- minimize the response time
- less memory consumtion or no memory leak

DevOpsEngineers:
Our main responsiblity is to release application from dev to prod as automated as possible
- fast releases
- less defects

As a devops engineer always look for best practices market our process should continuously evolve

1. Shiftleft[Do the security related scans in the dev env only instead of testing after production deployment]
2. DevSecOps[Implement the security every aspect of devops practice]
3. BuildOnceRunAnywhere[avoid it's working my machine problem by build immutable package run anywhere you want]
4. DRY[Implement reusability of ops][terraform-modules,jenkins-sharedlibs,helm,charts]

We should have meeting with developers
1. Which Programming laguage they are using and it's version
2. Deployment platform to host app
3. Discuss about branching strategy and merging [prefer feature branching always for agile]



setup:[ProjectOnboarding]
- repos creation and branch protection rules,PR process,approvals
- Folder creation for their project and RBAC
- Sonarqube project,create quality qates, prefer zero-tolerance no vulnerabilties,no code smells and security rating A mim 80% code coverage
- Nexus create a repo and policy
- AWS[vpc,...]

We can explain about our process and pipelines we are using shared pipelines these are centralized pipelines we have different combinations
- Java[EKS]
- NodeJs[EKS]
- Python[EKS]

If they are new platform we can provide the solution

We keep just simple pipleline that can at runtime whenever a developer pushes new commit we trigger pipeline  through webhook

1. Development pipeline
- checout
- unit test, owasp 
- scan
- docker build
- docker scan
- docker push
- deploy to dev
- functional we configure as per QA team inputs we can use selenium
2. Once the dev is success they raise PR get the approvals if it is approve they can move app from dev to higher env [QA,UAT]
3. Non-Prod fetch the image do the deployment integration testing 
4. We have CR Process App support team raise PR all stake holders,mention the time test results scan results etc..
5. Once CR is reviewed and approved app support team triggers PROD pipeline
6. Prod pipelins fetch the image and deploy
7. Once everything goes well app team perform sanity testing

continuously we check industry practices and try to implemnt after microsfot outage.We will review all our vendors process we checked our integration testing process agina with our vendor releases


Monitoring:
- Logs monitoring
- server monitoring
- app monitoring
- traces 

When app team upgrades the language version we check pipeline  once again



My day to day includes 3 things setup,ops,improvements as part of setup we need get jira tickets as part of ops we ensure smooth running of pipelines 
any version upgrade in app we test the pipelines end to end
as part of ops multiple clusters in Dev,Qa,Prod we do the upgrades using blue green deployment we make sure our cluster is secure we have continuous meeting with developers if they are facing any errors related to deployments like crashloop,pending,networking issues 

We get work through Jira Tickets our team leader assign tickets to us 





# During K8s cluster upgradation we have 10 worker nodes in order cordon or drain nodes we use manual approach what if we have 50 or 100 nodes this manual approach is tedious i feel