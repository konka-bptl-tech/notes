Developer: writes the code
Qa: they do different kind of testings once QA certifies the product we deploy the application in staging
Stage Environment: Testing based on how customer can would access this is called user acceptence test
Production Environemnt: Once UAT happens successfully then deployed into prod where end users can access product

Why do we need continuous delivery require for releasing our application

Continuous Delivery: Is a practice where code changes are automatically prepare for release to production

Entire continuous delivery divided into 2 process

CI/CD is a process to implement conitnuous delivery
- Genarlly CI/CD pipelines are decoupled

build: Take source code test it and create artifact

Traditional Builds: For example we have 3 developers write the code and test it locally and push to central repo. When Qa want to test usually QA wait some time to accumulate all the changes different developers done we take the code at some point of a day we go had a build generally called as nightly builds what happens in this method of building application once in a while we do a build if the build is Success we give in to the QA what happens if build fails there could be any one of the developers code one developer do a unit tests when club the all developers code might not work So build failed we need to ask QA to wait because build was failed So continuous testing not happen this is one of the problem and the next problem is we need to ask developers to fix the code which developer to ask in the matter of 5 to 6 hours there might be a 10 to 15 commit ids multiple developers modified the code which developer go and ask that exactly when it will takes lot time for us to debug and findout which exact code has problem when we follow this approach
- we would take lot of time to identify problem or bug
- QA has to wait
- Our continuous delivery interrupted
- Finally releas will be delayed

Our interest is to overcome these lapses to enable continuous delivery

As soon as developer completes coding we should be in the position to test it is there is an issue we should immediatly notify the developers 

This is where market comes with an solution that is automating build process i.e called CI

With CI
If Developers modfies the code and push into central repo we immediatly build if success give it QA if fails notify the developers they working and fix it again build process started automatically 

code should not have problem if problem take it and fix it this called continuous development
CI improves - find bugs faster,faster releases,improves quality

Applying security on top of the devops practices that becomes devsecops

Any pipeline by default starts with
1. Checkout code
2. Build and Unittests
   Why do we do unit tests in Pipelines evethough developers did in his machine he did only what he developed for example if he develops 2 functions and he tested only 2 so we can do unit tests for entire code
3. Code Coverage
   - how many lines of code tested
   - unused code
4. SCA
5. SAST













CI:  
CD: 
