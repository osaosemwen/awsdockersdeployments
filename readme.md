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
  


  
##### References
- https://docs.docker.com/get-started/
- https://blog.docker.com/2015/11/deploy-manage-cluster-docker-swarm/
- https://docs.docker.com/network/overlay-standalone.swarm/#run-an-application-on-your-network
