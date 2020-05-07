**JENKINS PROJECT**

**Introduction**
The objective of this project is to create two separate environments for Production & Testing. The Git repo shall have two branches - The Master branch & a side branch. In my case, I have named the side branch as dev1. There would be two web servers running using docker containers. The main production server would be exposed to the outside world. The other test server would be available on the system locally. Whenever a code segment would be committed on the Git Master branch, the post-commit hook would work to automatically push the code, thereby making things easier for the developer. As soon as the code would be pushed on Github, Jenkins would download the code & deploy it on the Production Server. The side branch dev1 would be used by the developers for testing some modifications. As soon as any block of code would be committed on the dev1 branch, it would be pushed automatically using the hooks. Then, jenkins would automatically deploy that code on the test server. If the Quality & Assurance team finds that the code is fit to be deployed, the team would run a pre configured Jenkins task which would merge the two branches & deploy that code on the main Production Server. This whole process would be automated using Jenkins.

*Step 1* :  Launch 2 docker containers from the httpd image. One container is for Production Server while the other is for Testing Server. 
![Launching Docker Containers](/images/docker.jpg)


----------------------------------------------------------------------------------------------------------------------------------------



*Step 2* : Create a Jenkins task for Production & give the repo link. It should be scheduled to run every minute so that any new code is automatically deployed on the Production Server.
![Configuration of Jenkins task for Production Server](/images/prod1.png)


----------------------------------------------------------------------------------------------------------------------------------------


In the Execute Shell Section, give the commands to copy the downloaded code to the web volume folder that is linked to the Production Server container.
![](/images/prod2.png)



----------------------------------------------------------------------------------------------------------------------------------------

*Step 3* : Create another Jenkins task for the Test Server. Any new code pushed to the dev1 branch should be automatically deployed to the testing server.
![](/images/test1.png)


----------------------------------------------------------------------------------------------------------------------------------------


In the execute shell section, give the commandds to copy the downloaded code to the test volume folder that is linked to the Testing Server Contrainer.
![](/images/test2.png)


----------------------------------------------------------------------------------------------------------------------------------------


*Step 4* : Our third Jenkins task is the most interesting one. As soon as the Quality & Assurance team successfully approves the code deployed on the test server, this third task would run that would automatically merge the two branches & deploy the tested code on the  main Production Server. The Production Server is exposed to the external world. In this project, I have used the concept of Tunnel to expose this server using ngrok software.


----------------------------------------------------------------------------------------------------------------------------------------


In the third task, along with the repo link, we need to provide the GitHub ctredentials so that it is authenticated to merge the two branches on GitHub.
![](/images/cred.png)


----------------------------------------------------------------------------------------------------------------------------------------


After providing the credentials, we need to use the _**Merge Before Build**_ option and provide the name of the branches, so that it can merge the two branches together.
![](/images/mbb.png)


----------------------------------------------------------------------------------------------------------------------------------------


Use the _**Git Publisher**_ option to push after the merge has been done.
![](/images/gp.png)


----------------------------------------------------------------------------------------------------------------------------------------

Now, use the _**Post Build Option**_ to build the Production Task as well as the Testing task. Since the Braanches have been merged, the tested code would be deployed on the Production Server.
![](/images/bop.png)


Viola !! Our fully automated System is ready to use. 
