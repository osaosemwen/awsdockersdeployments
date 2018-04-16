"This application is principal to showcase how cloud applications can be scaled across different geographical regions using dockers first, in which ELB can be used in managing traffics across, it is pricipally with dockers, the writeup is adapted from a couple of docker documentations referenced below"

![dockerswarm on aws cloud simple usecase](https://user-images.githubusercontent.com/17884787/38647160-ecbde85a-3db8-11e8-849d-600f782d8dc2.png)

# Use Case 2: Deploy same application for production on AWS docker-swarm in one region and multiple AZ.

#### Prequisites:

- Follow previous repository https://github.com/osaosemwen/containerdeployments.git
- Get a free-tier account with AWS or AZURE
- Clone this repo using ``` $ git clone https://github.com/osaosemwen/awsdockersdeployments ```
- Lastly, create an account on docker hub. 

## Link your AWS account to Docker CLoud

This link is essential, as it makes monitoring the deployment easy. Although one can configure all this steps solely on AWS, or create a stack to perform all this process using CloudFormation, but for this case, I would go the simpler route, using GUI.
- Log in to your AWS account on the top left corner click on services then Under Security, select IAM. The IAM is very essential for Cloud administrators as well as Cloud Security Engineers, cause this is the service used to give and take permissions, roles, access to any other services on AWS it is it also empowers granting access to 3rd party,  as this is what will be needed in this step.
- Click on Roles, and then create roles as shown below:

![creating 3rd party roles](https://user-images.githubusercontent.com/17884787/38783367-0c9f04ec-40cf-11e8-9974-d062e649d276.png)

- Next select the tab for Another AWS or 3rd party permissions as shown below:
  
![creating 3rd party permissions](https://user-images.githubusercontent.com/17884787/38783398-91ae98f0-40cf-11e8-88d2-78fa9a85673a.png)

- Docker cloud has an AWS account, that account has an ID 689684103426. Hence, This is the ID to be entered in the Account ID field 
- Select the option Require external ID (to assume role), the external ID will be your username in your docker cloud account, if you put a wrong user name this will results in "insufficient permissions" or "Invalid AWS credentials", Ignore the require MFA option. My user name is osemike hence your window should look similar to this shown below

![external id](https://user-images.githubusercontent.com/17884787/38783510-f50c58a0-40d0-11e8-8e0d-459093366f5f.png)

- Click Next permissions
- Click Next Review again (for now do not attach a policy to the role, this will be done later)
- Give the role a name and description
- Click Create Role, you will be returned with the role list of roles initially created.
- Click on the role you just created.
- Notice the ARN number, as this would be used for later.
- Below right Click on "Add inline policy" as shown below

 ![inline policy](https://user-images.githubusercontent.com/17884787/38783812-97074d32-40d5-11e8-8ab2-78d9866a53ae.png)
 
- Click on the JSON tab. 
- Copy and paste the JSON AWS IAM permission found from this link ``` https://docs.docker.com/docker-for-aws/iam-permissions/ ```  to the space available as shown below 

![json policy](https://user-images.githubusercontent.com/17884787/38783894-0740b2a4-40d7-11e8-836c-88705a926228.png) 
 
- Click Review Policy and Create policy

- From the Role List Click the role you just created and copy the Role ARN, it is the first line on the summary page  something like arn:aws:iam::0123456849229485i:role/name-of-newrole

## Adding your AWS account to Docker Cloud

After completing the steps listed above on your AWS account, and succesfully attaching an inline policy to the IAM new role as explained above copy the ROLE ARN string.
Go to your Dockers Cloud account.
- Log in and select Swarm mode
- Click on the "+" top middle then Swarm, or Click on Swarms then click on Create.
- Click on the cloud provider as shown below, It should both show not conected (Although mine shows connected for AWS, This is because I am done this process earlier).

![selecting cloud provider as well as regions](https://user-images.githubusercontent.com/17884787/38784267-5ed6c200-40dd-11e8-90a4-1f5ee7f98f64.png)

- On selecting AWS, you will be prompted to enter the ARN string you copied from the previous section from the IAM policy role you created, as shown below.

 ![aws credentials](https://user-images.githubusercontent.com/17884787/38784323-2bdc13fe-40de-11e8-82a7-690c6f476fc7.png)

- Click Save, The plugin icon shown glow which means you are can now deploy a swarm from docker cloud to your AWS account, as shown below.


### Creating A Swarm 
  
- This step is similar to the previous step only this time you would first give your swarm a name and select the already connected Cloud platform (AWS), select a region, as shown below, 
 
![swarm name](https://user-images.githubusercontent.com/17884787/38786675-6484ff40-40f7-11e8-94a9-074e49ed9f7b.png)

- Next Select a region (the region close to me is N.Virgina), click on Region Avanced Settings, select a VPC, a VPC CIDR, and a public subnet for the VPC, this could be essential for both security and to be specific in your cloud deployment, as shown below

![region settings](https://user-images.githubusercontent.com/17884787/38786680-687402fe-40f7-11e8-8659-fc62b595a89b.png)

- Select for now one noe for manager and 3 for clients just as used in previous repository
- Select an existing ssh key which you have for EC2 access. If you dont, go and create one you willbe prompted for it when you attempt launching EC2 instance as shwon below

![swarm and ssh](https://user-images.githubusercontent.com/17884787/38788272-cced8300-4100-11e8-97b3-9104b4d58ab0.png)

- Select Use Cloud watch for container logging
- Leave If you want to maintain free tier standard leave the Swarm properties as it is and click create Swarm

![swarm manager properties](https://user-images.githubusercontent.com/17884787/38787783-2858b4a6-40fe-11e8-95fa-30eebe648284.png)

- This will initialize a cloudformation stack on your AWS account which would create a swarm of dockers on your platform.

- To conect this swarm to your local terminal, when its highlighted green, confirm all the checks are completed on your EC2 platform in AWS the click on the swarm you just created. (Note you can If you select to be notified, and you provide your email, Dockers Cloud would automatically, initiallize an SQS for you to notify you of anything going on on your docker swarm)
- When the swarm you created is highlighted green, click on it, and copy the connecting code, required as shown below.

![connecting string](https://user-images.githubusercontent.com/17884787/38788826-2a4c23dc-4104-11e8-90e4-5550d095931d.png)

- Run Docker on your local machine and login in to docker hub using ``` $ docker login -u osemike ```, replace osemike with your user name.  
- Copy and paste the connecting string on your local terminal, this should connect you to the swarm directly, as shown below

![swarm connected](https://user-images.githubusercontent.com/17884787/38789021-689521ba-4105-11e8-8963-c08177b3a861.png)

- Run the export command as given ``` $ export DOCKER_HOST=tcp://192.168.99.100:32769 ```
- Where you can run ``` $ docker node ls ``` and ``` $docker ps ``` there after. you should have something similar to these

![docker node ls](https://user-images.githubusercontent.com/17884787/38789582-394c9b46-4109-11e8-9f5c-7564b1ee80a4.png)

and 

![docker ps](https://user-images.githubusercontent.com/17884787/38791871-40c06e3e-4118-11e8-807a-1a343f10eb44.png)

- After these you should be able to run your stack directly on the swarm 

``` $ docker stack deploy -c docker-compose-3.yml mikedockerpractise ``` 
---- 

Please Note recently, this connection give problems, on some computer If you have issues, here is a work around the problem.

![error in connection](https://user-images.githubusercontent.com/17884787/38789159-693f5cce-4106-11e8-95bd-60f380f63543.png)
 
### A work around deployment

- ssh to you cluster manager using the following 

``` ssh -i <path-to-ssh-key-used-during-cluster-creation> docker@public-dns-for-swarm-manager> ``` For example 

- ``` ssh -i aws-CF-practice.pem docker@ec2-34-214-107-204.us-west-2.compute.amazonaws.com ``` , make sure the ssh certificate has authorization +400 ``` chmod 400 aws_CF_practise.pem ``` this so because I made sure the ssh certificate is in the same folder as my PWD. 
- You should have a welcome message to docker !!
- run your defaults probing commands as usual as shown below

![docker node ls](https://user-images.githubusercontent.com/17884787/38789582-394c9b46-4109-11e8-9f5c-7564b1ee80a4.png)

and

![docker ps](https://user-images.githubusercontent.com/17884787/38791897-74408aa0-4118-11e8-8887-6bba0c867c8a.png)

When you enter ls, you have a blank env, This is the problem in doing it this way is that, it becomes a little difficult to get your code to the swarm manager.
----

#### Getting your code into Docker environment. 

- Create an image of an application using your most one of linux platform, I chose Ubuntu. 
- When you enter ``` $ ls /var/run/``` you would see you have the text editor vi available. Hence use it to create a Dockerfile. ``` $ vi Dockerfile ``` 
- In your Dockerfile create a redundant yet stable image where you would install git, and use it to clone this repository in the container. 
from these container you can now copy the repository to the PWD of the docker, I chose to use these

![external dockerfile](https://user-images.githubusercontent.com/17884787/38792039-159e1200-4119-11e8-9e5b-44d2af012fbf.png)

- Build the image using

``` $ docker build -t redis-server . ```

- Run this image using 

``` $ docker run --name redis_instance -t redis-server ```   

you should have something similar to the figure below, but when you use -d after the run above it runs in the backgorund

![redis image](https://user-images.githubusercontent.com/17884787/38790105-cfcf8fc6-410c-11e8-9c3e-4e0227c8ab1c.png)

- Copy the container ID of the image you just built from running ``` $ docker ps ```

- Access the container using 

``` $ docker exec -it df01dbe0d2a0 bash ```
- enter ls you would see the files available on the container including the file from the repository

![ls from container](https://user-images.githubusercontent.com/17884787/38790625-881a7b2e-4110-11e8-83ca-4330e67ae3c3.png)

- Exit the container
- Now we can simply copy the cloned repository from the container to the Docker env itself using 

``` $ docker cp <container ID>:<source-folder/file> <destination-folder/file> ``` For example

``` $ docker cp df01dbe0d2a0:/containerdeployments . ```
- Now cd into the cloned directory
- and run the command to deploy your stack on your AWS CLOUD enviroment !!

It is important to make create the ./data folder as created in the previous application deployment on the docker machine.

else you would have something similar to picture shown below:

![redis counter application](https://user-images.githubusercontent.com/17884787/38791265-b255d1b4-4114-11e8-9e11-de11e9bc1eee.png)

- copy your Public DNS from the any of the instances deployed, using the same port number as before "5050"

check your application on the cloud, you should have something similar to what I have below

![deployed app on different az](https://user-images.githubusercontent.com/17884787/38791388-58a3d41c-4115-11e8-8df2-66c0044b16f8.png) 

- In practice in is important to make use of DNS, and add further security on the TLS. Sticking to the initial diagram, we have sucessfully deployed same application on one region spreading it across different Availability Zones.

# Deploying the your App on Different Regions

There are two ways of doing it, it either create similar swarm on two regions and use ELB to handle traffic accorss this regions or you ssh from a different region and connect to the swarm and deploy your stack over these regions.

If anyone really wants this please send me an email, and I would create the repo. 

### That is IT ENJOY !!!!



### Clean-UP !!! 

- This is very important as if its only for practise, creating and running a swarm will incur charges if left to run over time. 
- Go to your Docker cloud, on your right corner click on the 3 dots for your stack
this will automatically erase everything.

- Click delete. Alternatively go to your AWS Cloud Formation and delete the stack, 






  
##### References
- https://docs.docker.com/get-started/
- https://blog.docker.com/2015/11/deploy-manage-cluster-docker-swarm/
- https://docs.docker.com/network/overlay-standalone.swarm/#run-an-application-on-your-network
