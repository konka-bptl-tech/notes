Dasher technolgies is a software provider which possibility of transitioning  leverages

Jenkins is a opensource automation server that accerate software development it orchestrates entire software delivary pipeline from building testing and deployment CI/CD pipelines quickly identitfies code issues enabling faster development and higher quality software 

developers regulalry push their code to repo webhook triggers jenkins to executes CI/CD pipelines it will executes sequence of steps if anything goes wrong developer quickly notified developers works on the error again push to code to central repo again pipeline started

This loop keeps a code fresh minamal manaul intervention the process is keeps on repeating itself ensuring smooth and automated development workflow

- Job
- builds
- freestyle job
- pipelines
- stages
- nodes[controlplane+build-agents][controlplane responsible to coordniating builds,distribute the tasks to build agents]
- plugins

pros:
- opensource
- highly cistomizable software
- using groovy scripted pipelies write advanced[complex-workflows] pipelines
- using pipelines as a code

cons:
- maintenance overhead
- performace issues
- security take

DevOps:

DevSecOps:

Need Of CI/CD:

Jenkins levelrages distributed architecuture to efficiently manages CI/CD pipelines efficiently this architecutre offers scalability allow you automate complex workflows across multiple machines

control plane = Responsible for Coordninating entire CI/CD pipeline
- `Management Tasks` plugins installation,user auth&autho, integration with others, secrets management
- `Job Management` define,scheduling and monitoring jobs

Build Agents = Responsible to excute pipelines

Types of Projects:
1. Freestyle
   - Using Web interface configure the Jobs
   - Limited workflows
   - Non code based configuration
   - Limited Functionality
   - Cannot resume after failure
2. Pipeline
   - Writes pipelines as code either declarative syntax or groovy[scripted] syntax
   - versioning pipelines and improve the collaboration

3. Multi Branch pipeline
4. Maven Project
5. Multi Branch Configuration
   - 
6. Organizations Folders
   - Organize Pipelines in highrarchy structure

Plugins:
- copy artifact plugin
- yet another build visualizer
- Blue Ocean
- RBAC
- Mock Security realm
- matrix authorization strategy
Jenkins finger print

if not variable substitution use single quote if you have variable sunstituion use double quotes

Automating Jenkins using CLI and API

Jenkins CSRF CRUMB 

pipeline script from scm

Why do we need parametrize builds
In Jenkins a parametized builds are used to create flexible and customizable builds jobs by defining parameters that can be passed while triggering a build this parameters can be used to make your builds more dynamic 

BRANCH - String Parameter
APP_PORT - String Parameter
SLEEP_TIME - Choice parameter

Branch Specifier
*/${BRANCH_NAME}









-----
Conitnous Delivery = Where code changes are automaticaly prepared for a release to prod

Build = take source code and convert into a single artifact which can be convert into QA ot customer

-------------------------ADAM CI/CD
1. One of the common task that would you get or one of the imporatant that we have to help an organization or a product is setting up CI/CD pipeline 
2. General workflow any workflow in any organization so what happens first developer writes the code that would be given to QA once QA certifies the product deploy the application into staging environment this is where we test based out of how the customer would access called user acceptance test once this is done we put our product on environment called production  this is where customers will access.
3. How ever the market moving faster so what exactly we need continous delivary of your product
4. For delivering product continously the work starts with developer where continuously with whatever the requirement they get developers have to write code or the program and then equally that code has to be tested continuously by QA what ever code that has been certifes by QA we need to put it or deploy it into staging and production environment that exactly what customers will be accessing so this process where the developers continuously work followed by QA continously taking that the testing and then code which QA has certified we continuously deploy it into production So that customers can access this is exaclty called ad continuous delivery
5. Eventually entire Continuous delivery process can be divided into 2 categories that is CI and CD
6. CI/CD is process which help us implement continuous delivery
7. We need to automate 2 halfs so the one half is automate build and another half is automating CD pipeline
8. Problem with Traditional Builds when a developers develope the code and they can test the code locally like unit tests and then commit and push from their local repo remote repo same every developer go through the same process but what happens when QA wants test QA waits for some time to accumulate all the which different developrs are done and then we code at some point of time in a day and we go ahead and do a build now this way building is what we generally call it is nightly builds 
9. So what happens in this method once in a while so lets assume once in 5 to 6 hours you do a build if that build is success we give it to QA but what happens if a build fails now why would a build fails because there could be a problem in any one of the developers code or the developers has modified and he has tested his code which is called unit testing but when multiple codes are combining sometime might not work and that's exactly when what happens is when we do a build and if the build fails for any reason then that probelm that we have is we need to to ask QA to wait because we don't have successful build we don't product or artifact created so QA has to wait if QA waits your continuous testing not happens if QA does not testing so we can't make CD so this one of the problem
10. The next problem is we need to ask developer to fix the code which developer will you ask because in matter of 5 to 6 hours developers commiting 10 or 15 commits there would be multiple developers are modified the code so which developer will go and ask and that exactly when takes lot of time for us to debug and findout which is the exact code which is having the problem now overall when we follow the traditional method what happends is we take lot of time to identify the problem to debug and at the same time QA has to wait and it will cost us continuous delivary to be interrupted you release will be delayed
11. This is when out interest to overcome and to enable continuous delivery which means has soon as developers completes his work we should be in a position to test it or there is a issue should able to immediatly tell the developer you code has problem please fix it and this is where the market came up the new process of autmating the build or the build pipeline this is what we called CI
12. `CI` is a process in which instead of waiting some time as soon as developer modifies and pushes code to remote repo we immediatly build and we check wheather we can give that build to the QA if there is an issue when build fails ask the developer to fix
