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


-------------------------------------
we are working as a devops engineer one of the common task that you
would get or one of the important automation that we have to help an
organization or a product is setting up cicd pipelines right and that's also
going to be one of the common question that anyone would ask you in the interview so being that uh this session
uh would be mainly focusing on what exactly is the cicd pipeline and in
terms of security how do you implement the def secops into the cicd okay so
that way let's Deep dive into the concepts about what's exactly CI what's
CD and how do we add the security which is the depic Ops towards it okay so to
get started let's kind of first understand uh in in a general workflow in any organization so
what happens first developer writes a code and that would be given to QA where they
do different kind of testing and once QA certifies the product we deploy that
application into a environment called staging environment and this is where we
test based out of how the customer would access which is what we call it as the user acceptant testing and once this is
done then we put our product into a environment called production and this
is where customers will access however with the market moving very
faster so what exactly we need is is a continuous delivery of your product
whether you take Facebook LinkedIn or any banking site or your shopping
website or any e-commerce site so we need the delivery of the products continuously on our web page right so
for this what we have to understand is almost everyone in the organization has
to continuously work which means for delivering a product continuously the
work starts with the developer where continuously with whatever the requirement they get developers have to
write the code or the program and then equally that code has to be tested
continuously by QA and whatever the code that has been certified by QA we need to
continuously put it or deploy it into staging or production environment and that's exactly what the customers will
be accessing so this process where the developers continuously work followed by
QA continuously taking that and testing and then the code which QA has certified
we continuously deploy it into production so that customers can access now this is exactly what we call it as
continuous delivery now however if you look closely so continuous delivery is a
practice which is when whatever the code which developers continuous ly modify
depending on the requirement we test it and we make sure that code is ready for
production and eventually your entire continuous delivery process can be
divided into two categories so that is CI and CD
so the automation or the work which we continuously do to
integrate the development and the testing is what we call it as Ci or
continuous integration and at the same time the code that has been certified by
QA how do we make sure that it is put into the staging or production environment so that the customers can
access is what we call it as continuous deployment so basically when I say cicd
cicd is the process which helps us to implement continuous delivery so that
right from the beginning how a developer modifies the code you take it and hand it over to QA and QA can test it and
once the QA tested we put it into production right so this is when for us
implementing continuous delivery is where we kind of implement your cicd so
cicd is needed because every company is focusing towards continuous delivery so let's be clear on that first on why
exactly do we need cicd but still if we take one more step so most of the time
when you go out and see any of your YouTube videos or any Google right uh
notes usually when we say cicd it's not a single stretch of automation where we
build a test and put it into production so you have to divide or ideally it is
divided into two parts of automation especially when we are trying to automate as I was showing in the
previous slide we need to automate it into two halves so the one half is
automating the build which is where as soon as a developer completes his work
how do we take that code convert it into a product and how do we give it to QA
right and at the same time when I have a product how do I make sure whether if I
want to deploy it on a QA environment or on a staging environment or a production environment I need to have the ability
so that's when your cicd is ideally not a single stretch of automation but it is
kind of divided into two parts one is the build Pipeline and the deployment
Pipeline and that's where when we are adding security and also automating this
end to n cicd it is important for us to understand each part separately and then
how both of them are connected to form a end to end pipeline which is what we call it as cicd all right so now let's
get into each part one by one so first let's understand how do we automate and
secure your build Pipeline and then we'll understand how do we automate and secure your deployment Pipeline and in
the dep secops how do we connect these two to form your n2n CD pipeline all
right so now as most of you understand so when
I say a build a build is a process where you take a program or a source code of a
program and you convert it into a product or artifact which can be
consumed by QA or customer but in a traditional method what happens is well
now assume you have a bunch of developers where we store all our code
into a server like gitlab or GitHub now eventually when a developer modifies the
code on his local workspace or on his workstation he or she will do a unit
test followed by if that is fine then they will go ahead and commit and push
the code from their local report to remote repository the same way every
developer will go through this process but what happens is when we want
QA to test so usually we wait for some time to accumulate all the changes which
different developers have done and then we take the code Cod at some point of
time in a day and we go ahead and do a build now this way of building is what
we generally call it as nightly bills okay so this is one of the traditional method which we were following now what
happens in this method once in a while so let's assume once in 5 hours or 6
hours you do a build and if that build is Success we give it to QA but what
happens if a build fails now why would a build fail because there could be a
problem in any one of the developers code or the developer has modified and
he has tested his code which is called as unit testing but when multiple codes
are combining sometimes that might not work and that's exactly when what happens is when we do a build and if the
build fails for any reason then the problem that we have is we need to ask
QA to wait because we don't have a successful build and we don't have a
product or artifact created so QA has to wait now if QA Waits what will happen
your continuous testing is not happening right if QA doesn't complete the testing
you will not be even going to the next step which is called a CD or continuous deployment
so this is one of the problem the next problem is okay we need to ask
developers to fix the code now for a developer to fix the code which developer you will ask because in a
matter of 5 to six hours there might be 10 or 15 commit IDs and there might be multiple developers who would have
modified the code so which developer you will go and ask and that's exactly when it takes a lot of time for us to debug
and find out which is the exact code which is having the problem now overall when we follow this traditional method
what happens is we take lot of time to identify the problem or a bug and at the same time QA
has to wait and this will cause our continuous delivery to be interrupted or
in other words you will not be having continuous delivery your release will be delayed and this is when our interest is
to overcome these issues and to enable continuous delivery which means as soon
as the developer completes his work we should be in a position to test it okay
or if there is an issue we should be able to immediately tell the developer that your code has a problem please fix
it as soon as possible and this is where the market came up with a new
process of automating the build or the build pipeline which is what we call it as continuous integration okay so let's
first understand that so what is CI or continuous integration so it's a process
in which instead of waiting for some time as soon as a developer modifies his
code and pushes it to the remote or the central repository we immediately build
it and we check whether we can give that build to the QA or if there is an issue
and build fails ask the developer to immediately fix and if you try to see
how it works so the same way now when a developer modifies the code
immediately we do a build okay and when this build is Success we give it to QA
and if it fails we go back and immediately tell the developer that hey your code which you modified is not
working fine that could be for a unit test it is not working or while the code
got integrated with other code it might not be working for whatever reason if the build fails immediately we go back
and tell the developer so this way as soon as a developer modifies the code we
will have the ability to create a product or do the build and tell if the
build is success I can give it to QA so this ensures there is a continuous
development because what is continuous development it is not that developer will just write code and throw it off
well the code that he has given should not have any problem if it has problem he should take it and immediately fix
and once again give it back that's what we call it as continuous development so in this method developers will be able
to continuously develop and they will know in case if there is a problem you
can find it out immediately so continuous integration is a process in
which the developers will be able to find out if there is a problem
immediately because as soon as the developer modifies the code and pushes the code to the remote repos itory
immediately we do the build and we tell if it is failing who is causing the problem and the same way now since if
one developer push the code and if it is not working fine immediately there will be one more developer who is modifying
the code and if his code is successfully built QA can take it and go ahead to the
testing so that way they don't have to wait correct so the earlier problem problem whichever was introduced by a
developer one he will be fixing the code whereas the second developer whose code is working fine we can go ahead and
further do the testing so that way continuous development will be happening
continuous testing will be happening and the same thing can be delivered so you will be able to reach faster releases
and in other words continuous integration helps us to drive the
continuous delivery process okay and not only that it helps us to improve the
quality because instead of accumulating all the code and doing testing in one go
as soon as you get a code if you are able to test it immediately if there is any bugs we will be able to find and fix
it so that way with the help of continuous integration you will be able to do faster releases and the quality of
your application or product would be very very high okay and not only that in
continuous delivery CI is your most important and the first part which
enables us to drive so that way let's first understand now if you are going to
set up your cicd pipeline the first pipeline that you have to set up is automating the build in which whenever a
developer pushes a code to a particular repository you should be able to immediately
trigger a build and tell the developer whether it is good or it is not good all
right so now let us get into the pipeline stages
okay and ideally well we are not just focusing about a regular CI build or a
regular build pipeline but we need to implement a security towards it so that
is where applying Security on top of anything becomes depops okay so now our
focus is how do we automate a Ci or a build Pipeline and in that we are trying
to see how do we ensure more and more security can be implemented and that's where in a real time okay there are
close to nine stages which we have to think about implementing to make it a
successful depth secops build pipeline