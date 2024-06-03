# Cloud
Cloud Basics
AMAZON WEB SERVICES
1.	Elastic Compute Cloud(EC2)
a)	Go to Ec2 dashboard
b)	Go on instances and click create instance
c)	Give name,select AMI,select type as t2.micro(free).
d)	Select existing or create a new Key pair.If creating new give it a name and click on create key.
e)	Keep network setting as default.
f)	Allow SSH traffic from everywhere (0.0.0.0/0)
g)	Launch the Instance.
2.	To access Windows EC2 instance
a)	Using ssh command 
i.	Open cmd in directory where your key value pair is downloaded
ii.	Type “ssh -i {key-pair name}.pem {public-DNS || username@public-IP}”
iii.	Now press enter and now successfully you have connected with your EC2 machine
b)	Using Remote Desktop Connection
i.	Open Remote Desktop Connection app and enter your public IP
ii.	Enter the username and password provided in the EC2 console
iii.	Now click on connect and now successfully you have connected with your EC2 machine
3.	To access Linux EC2 instance
a)	Opne CLI in linux where key pair is downloaded
b)	Change permission to give access of reading the key pair to owner only-> chmod 400 {key-name}.pem
c)	Now type the ssh command 
4.	To install some software on Linux EC2 insatnce
a)	Open the instance and click on connect to instance and then finally on connect
b)	Become root user-> sudo su
c)	Update apt-> apt update
d)	Install software-> apt-get install {software name}
5.	Amazon Machine Image
a)	Select your instance.Go to Actions and click on create image and template
b)	Give name to your image
c)	Click on create image
d)	Your AMI is created.
e)	To view it go on Your AMIs
f)	Hurray your AMI is created
g)	Now to create instance from this AMI
i.	Select the AMI and click on Launch template from AMI
ii.	Select key pair
iii.	Click on Launch instance
6.	Create Load Balancer
a)	Go to auto Scaling groups
b)	click on Create auto Scaling group
c)	 Given the name of auto scaling
d)	 select an existing launch template or create a launch template
e)	   Create launch template
i.	Given your template name and template version description
ii.	 Choose your Amazon Machine Image
iii.	 Select instance types
iv.	    Create a new key pair for login
v.	 Go to Network settings and configure security group
vi.	Configure Storage
vii.	 Review your instance
viii.	Then Click on Create Launch Template
f)	 Select launch template and click next
g)	Choose instance launce options
h)	choose Availability Zones and select 2 or more
i)	 Configure load balancing
j)	choose value for Health check grace period and goto next
k)	 Configure group size and scaling
l)	 Choose your Desired capacity 
m)	 Configure scaling
	   1  set Min Desired capacity
         2  set max Desired capacity
n)	 choose instance maintenace policy  and go to next
o)	Click Create auto Scaling Group
p)	go to EC2 Dashboard and click instance
q)	connect with IPV4 public Ip
r)	write the command on console
sudo su
yum update
yum install stress
stress -core 10000
7.	S3 Bucket
a)	click on services at left corner and selecting the S3 from list of services
b)	Click on create bucket
c)	Choose AWS Region
d)	Then Select the Bucket type and choose option general purpose
e)	Given the unique name of bucket 
f)	 Configure public block access setting
      if you want to give permission to access public then remove checkbox
otherwise click on chechbox
g)	 Configure bucket Versioning and select enable
h)	now create bucket
i)	After That give Configure Bucket policy
     - click on edit button
- then click on policy generator
     - select the Policy Type and choose s3 bucket policy
     - Configure add Statements
         - choose principal *
         - and choose action :- GetObject
         - Put ARN :- arn:aws:s3:::abhigupta123/*
         - After that Add Statement
         - Click on Generate Policy
         - copy the code and paste on Bucket Policy
j)	 After that Click on Save Changes
k)	goto the bucket dashboard and click on the your bucket name 
l)	 click on the upload button and upload your file,photo,video,and other
m)	 click object link and also access your data from over internet through your object link
8.	Virtual Private Cloud
a)	Create a VPC
i.	Give name of vpc
ii.	Select some private IPv4 CIDR e.x 192.168.0.0/16
iii.	Click on create VPC
b)	Create Subnet
i.	Select VPC for which we want to create our subnet
ii.	Go to subnet settings give it name,select availability zone,enter IPv4 for this subnet(we’ll create 4 subnets e.x 192.168.1.0/24,192.168.2.0/24,192.168.3.0/24,192.168.4.0/24)
iii.	Now click on create subnet to create the subnet
c)	Create Instance
i.	Configure the instance as mentioned above in the notes
ii.	Edit network setting
-Select vpc for which you want to create this instance
-select any 1 of the subnet
-enable auto assign public IP
-create security group where ssh will be enabled
iii.	Launch the instance
d)	Now if we try to connect this instance we’ll not be able to connect, even if public IPV4 is assigned to the instance.
e)	Now create internet gateway 
i.	Go to Internet gateway in VPC dashboard
ii.	Click on create internet gateway
iii.	Give name to the gateway
iv.	Click on Create internet gateway
f)	Attach this internet gateway to our VPC.
g)	After doing this our VPC can access Internet
h)	Still we cannot connect to our VPC instance
i)	Now we’ll create route tables for our VPC
i.	Go to route tables on VPC dashboard
ii.	Click on edit routes
-Select 0.0.0.0/0 and select Internet Gateway
-Click on save changes
j)	Now if we try to connect to our EC2 instance, it will get connected
k)	The problem is the route gets attached to every subnet which we don’t want as we want to keep some subnet for private work where internet is not accessible
l)	To make subnet private
i.	Create one more route table with no access to Internet Gateway
ii.	Attach this route table to our subnet which we want to make private
m)	Now make private instance
	-select vpc with  private subnet and disable auto assign public IP.
-Add security group enabling ICMP with access to anywhere
n)	Now if we try to connect to this private instance we will not be able to connect to it.
o)	We can check connectivity b/w private subnet and public subnet by connecting public instance and sending ping to private instance.
p)	Since our instance is private and we cannot access Internet on it so we will not be able to install any software in private instance.
q)	Now we will try to access private instance using public instance
i.	Connect to public instance
ii.	Open your key attached to the public instance and copy it’s content
iii.	Enter command vi {file-name}.pem and press “i” to enter insert mode, paste the copied data  and save using wq.
iv.	Now changing permission of this file to read only for user -> chmod 400 {file-name}.pem
v.	To access private instance type-> ssh -i {file-name}.pem ubuntu@{private-IP}
vi.	Now we are in our private instance but still we will not be able to access internet on it.
r)	Now we will create NAT gateway which will help us in giving internet access to our private instance
i.	Click on NAT gateway on VPC dashboard
ii.	Give name
iii.	Select public subnet on which our public instance in associated
iv.	Click on Allocate Elastic Ip 
v.	Click on create NAT gateway
s)	Now we will attach this NAT gateway to our private Route by going on edit route and selecting Nat gateway on our target with destination as 0.0.0.0/0 
t)	Now we can successfully access internet on our private instance connected through public instance
u)	We can deny request from certain Ip’s  with help of Network Access Control Lists
i.	Go to Network ACLs in VPC dashboard
ii.	Edit the Acl assoictaed with our built subnets
iii.	Edit inbound rules
	-Try to give shortest possible rule number
	-Select all traffic
	-Source : IP address on which we want to deny the access
	-Choose Deny
iv.	Click on save changes
v.	Hurray you have successfully denied the access to a particular IP
9.	RDS (MySql and Sqlectron)
10.	Dynamo Db
11.	Image building and uploading using Docker on Docker Hub
a)	Go to Turn Window features on and off and turn on HyperV and Wsl.
b)	Install Docker and Login/SIgnup into your account
c)	Pull image(iso file) from docker hub(create account and get prebuild images of node)
i.	Open cmd
ii.	Type: docker pull node:18.20(to download image)
iii.	Type: docker images (to check your downloaded images)
iv.	Type: docker run -d node:18.20(to run image)
v.	Type: docker ps
d)	Open vs code
i.	Create folder with name node and in src directory add file index.js. Create DockerFile in same directory as of src
ii.	Inside index.js write 
const express = require('express');
const app = express();

app.get("/",(req,res)=>{

     res.send("Welcome to docker ....");

});

app.listen(3000,()=>{
    console.log("Server Started");

})
iii.	Inside DockerFile write
	   FROM node:21-alpine
RUN mkdir -p /home/app
COPY package.json /home/app/
COPY src/index.js /home/app/
WORKDIR /home/app
EXPOSE 3000
RUN npm install
CMD ["node","index.js"]
iv.	Now in VSCode terminal create new image using these two files
1.	Type: “docker build -t appName:v001 .”  .
2.	Type:”docker images” (to check your built image)
3.	Type:”docker tag satvik1608:v001 satvik1608/uca:v001”(to create new image for uplodation)
4.	Type:”docker push satvik1608/uca:v001”(to upload image)
v.	Now you can search and check your image 
12.	Kubernets
13.	Beanstalk
14.	CloudWatch(To stop Linux instance if CPU utilization is less than or equal to 15%)
a)	Create an EC2 instance
i.	Go to Ec2 dashboard
ii.	Go on instances and click create instance
iii.	Give name,select AMI(Linux),select type as t2.micro(free).
iv.	Select exisiting or create a new Key pair.If creating new give it a name and click on create key.
v.	Keep network setting as default.
vi.	Allow SSH traffic from everywhere (0.0.0.0/0)
vii.	Launch the Instance.
b)	Select EC2 instance and go to monitoring to view stats(Cpu usage,credit balance,etc)
c)	Click on manage detailed monitoring and enable it to view stats after every 1min (instead of every 5min)
d)	Copy Instance ID
e)	Go to services and search for CloudWatch and open it in new tab
f)	In dashboard navigate to All Metrics 
i.	Click to EC2 
ii.	Go to Per-Instance Metric
iii.	In search bar paste your instance id and click on search
iv.	Now select the option with metric name as CPU Utilization
v.	Click on “create Alarm”
vi.	Choose period to 5 min(itne time frame tk check krega)
vii.	Keep Threshold as static in conditions
viii.	Choose Lower/Equal in conditions
ix.	Give 15%
x.	Click on next
xi.	Now in actions click on Select insufficient data and click on create new topic
xii.	Give name to topic as “CPU-underload”
xiii.	Provide your email id in email endpoint on which you want to receive the SNS 
xiv.	Click on create topic
xv.	Choose your action as EC2 action
xvi.	Choose InAlarm and select stop instance click on next
xvii.	Select configuration and give name as LinuxServer
xviii.	click on create alarm
xix.	Go to your mail and confirm subscription in the email to turn on the alarm with SNS
15.	CloudFormation(using CloudFormation template for automatic creation of instance)
a)	Template(Code) upload->S3 Bucket<-reference Aws CloudFormation creates->Stack->Aws Resources
b)	Resources are mandatory for creation of templates
c)	Steps
i.	Navigate to AMI’s catalog and select the image id of desired AMI (ami-04e5276ebb8451442)
ii.	Create a yaml file for template and write(name will be MyInstance) 

iii.	Navigate to Cloudformation
1.	Click on create stack
2.	Choose prepare template as choose an exisiting template
3.	Choose upload a template and Upload the yaml file
4.	Click on Next
5.	Give name to your tag and click on next
6.	Keep tags,permissions and stack failure options as default and click on next
7.	Click on submit
8.	Now you can check in your EC2 dashboard your instance will be created

16.	AWS Lambda
a)	Create S3 bucket
i.	Give name
ii.	Remove block all public access

b)	Create Lambda function
i.	Navigate to Lambda dashboard
ii.	Click on create function
iii.	Select author from scratch
iv.	Give function name
v.	Chooose runtime env as Python 3.8
vi.	Click on use an existing role and select LabRole
vii.	Click on create function
viii.	In code section override with this code
	   import boto3
from uuid import uuid4
def lambda_handler(event, context):
    s3 = boto3.client("s3")
    dynamodb = boto3.resource('dynamodb')
    for record in event['Records']:
        bucket_name = record['s3']['bucket']['name']
        object_key = record['s3']['object']['key']
        size = record['s3']['object'].get('size', -1)
        event_name = record ['eventName']
        event_time = record['eventTime']
        dynamoTable = dynamodb.Table('newtable')
        dynamoTable.put_item(
            Item={'unique': str(uuid4()), 'Bucket': bucket_name, 'Object': object_key,'Size': size, 'Event': event_name, 'EventTime': event_time})
ix.	Click on deploy

c)	Create Dynamo DB table with a name as “newtable” and partition key as “unique”.
d)	Now create a Trigger 
i.	Go to created Lambda function and click on Add Trigger
ii.	Select S3 bucket
iii.	Choose your created bucket
iv.	Select event types on All object create events,PUT,POST,COPY and on All object delete events
v.	Tick the acknowledgement 
vi.	Click to create it
e)	Go to S3 bucket and upload any file
f)	Now you can check about the details of file uploaded in your dynamo db table newtable.

17.	AWS Key Management Services
a)	Navigate to Key Management Services
i.	Go to customer managed keys
ii.	Click on create key
iii.	Choose key type as symmetric
iv.	Select key usage as encrypt and decrypt
v.	Under advanced options choose KMS
vi.	Click on next
vii.	Give Alias to your key (ucakeys)
viii.	Click on next
ix.	Choose key administrators as LabRole
x.	Choose key users (user1,user2)(user1 will be given full S3BucketAccess Policy from IAM)
xi.	Click on next
xii.	Click on finish
xiii.	Hurray! Key is created.
b)	Login AWS account of user1
i.	Create S3 bucket with default encryption as server side enc with AWS key management service
ii.	Click on choose from AWS key and choose your created key
iii.	Click on create bucket
iv.	Now try to upload files in your created bucket
c)	Login your Administrator account
i.	Now choose your key and go to key policy
ii.	Click on “switch to policy view”
iii.	Scroll down to field where we have permissions of user1 and user2
iv.	Edit the policy and inside Action remove all actions
d)	Now if I try to upload a file from user1 , we will get an error because Access was denied
e)	Even if full S3 bucket access was given to user1 we will still be not able to upload as now policy is now controlled by KMS

18.	Directory Services
a)	Navigate to Directory Services
i.	Select AWS Managed Microsoft AD  from drop down menu
ii.	Click on create directory
iii.	Click on next
iv.	Select edition as Standard edition
v.	Give Directory DNS Name as www.mydomain.com
vi.	Enter Admin password
vii.	Click on next
viii.	Choose default VPC and any 2 subnets
ix.	Click on next
x.	Finally click on create directory
b)	Create a windows machine
i.	Navigate to EC2 dashboard
ii.	Click on instances
iii.	Create instance of windows with RDP traffic allowed
iv.	Click on launch instance
v.	Connect to your instance using RDP client
vi.	Download remote desktop file
vii.	Get your password
viii.	And connect to your RDP using the password
c)	Go to your search bar and search server manager on your machine
i.	Go to network and internet setting
ii.	Go to change adapter settings
iii.	Double click on TCPIPv4
iv.	Add the DNS address (copied from Active Directory 172.31.29.71)
v.	And click on OK
d)	Now change work group
i.	Go to properties from This PC
ii.	Click on rename this pc(advanced)
iii.	Click on change
iv.	Select domain and add your domain name
v.	Enter your username : admin and password(password will be the password you gave to your Directory Service)
e)	Now we can manage this instance through our directory

19.	Amazon Lex(to create chat based services and take data from user)
a)	Navigate to Amazon Lex services
b)	Click on create bot
c)	Select creation method as Create a Blank Bot
d)	Give name of Bot
e)	Choose create a role with basic Amazon Lex permissions under IAM permissions
f)	Choose Children’s privacy protection act as No
g)	Set 5mintues as Ideal Session Timeout
h)	Click on next
i)	Select languages
j)	Click on Done
k)	Now your chatbot is created
l)	Now choose Intent as choose conversation flow
m)	Choose sample utterances
n)	Add message group(first message your bot will send)
o)	Under confirmation add Confirmation prompt and Decline prompt
p)	Click on save intent
q)	Now click on Build
r)	Go back to Intents list
s)	Modify your created intent
t)	Under message group add message as “Can I know your name”
u)	Now go to Slots and click on add slot
v)	Now choose Name as “YourName” and Slot type as AMAZONFirstName and add prompt as “Can I know your name?”
w)	Click on Add
x)	Now go to confirmation and in confirmation prompt type “Thanks Mr/Ms {YourName}”
y)	Now save the intent,Build it and Test the chatbot.

20.	Creating custom variable slot in chatbot(giving menu to the user)
a)	Go to slot types
b)	Choose Add blank slot type
c)	Give name of your slot as drinkType
d)	Click on add
e)	Choose slow value reolution as “Exapnd values(default)”
f)	Under slot type values add Tea,Coffee,Cold drink and Cold Coffee as individual values
g)	Click on save slot type
h)	Now navigate to intent and select your created chatbot
i)	Scroll down to Slots and click on add Slot
j)	Choose your created Custom slot type, here “drinkType” and give name as “drink”
k)	Add prompt as “Drinking options available are Tea,Coffee,Cold Drink and Cold Coffee”
l)	Click on add slot
m)	Under confirmation modify your previous message to “Thanks Mr/Ms {YourName}, your order of {drink} is confirmed”.
n)	Click on save intent
o)	Build the chatbot and now click on Test to test it

21.	 Steps to launch windows instance using EC2
1 - Sign in to AWS management console
2 - navigate to the EC2 dashboard
3 - Click on Instances in the left navigation panel.
4 - Click Launch Instances.
5 - choose an AMI (Amazon machine image): For Linux, select an AMI such as Amazon Linux 2 or  Ubuntu. For Windows, select an AMI such as Microsoft Windows Server 2019 Base.
6 - choose an instance type
7 - configure instance: config. details like no of instance and network settings.
8 - Add storage
9 - Add tags - to organize and identify
10- config. security groups - create security groups or choose existing security groups
11- key pair selection - select existing or create a new key pair. This is required for accessing your   instance via SSH. (RDP for windows)
12- Review instance launch
13- Launch instance
14- Launch status - After launch you will be directed to instance view then you can see the status of your instance. Once it is in running state you can further proceed to connect to it.

22.	Detailed steps to create an application load balancer in the AWS management console
1 - sign in to the AWS management console.
2 - navigate to the EC2 dashboard: once logged in, go to the EC2 management console & navigate to the EC2 dashboard by clicking on 'services' at the top corner & selecting EC2 from the list of services.
3 - create two instances of amazon Linux with SSH(22) & HTTP(80) port accessibility.
4 - create a load balancer : In EC2 under load balancing click on load balancer and then click on  create load balancer button.
5 - Select load balancer type : choose the application load balancer option and click on create under the create load balancer section.
6 - configure load balancer settings: such as provide name for LB
7 - configure security settings: Select an existing security group or create a new one for your load balancer.
8 - configure routing: define the target group for your LB.
9 - Review & create: review the config. settings you have chosen for your LB. Once you have reviewed everything then click on the create button to create the LB.
10- Wait for provisioning - AWS will now provision your load balancer, which may take a few minutes then you can track the progress on the load balancer page.
11- Access LB details: once the load balancer is created successfully you can access its details from  the LB page in the EC2 dashboard.


23.	Kinesis: Use two lambda function (Producer and consumer) to generate data through Kinesis  and put lambda function in S3 bucket :-
1 - (a) go to lambda function
     (b) click on create function
     (c) give name to the function
     (d) Select the role of the function and use existing role.
     (e) select the lab role.
     (f) then click on create function.

2- (a) now go to kinesis service
     (b) click on create data stream.
     (c) give a name to the data stream.
     (d) rest setting will be same.
    	     (e) then finally on create data stream.

3- (a) go again to lambda function 
    (b) write source code which will automatically produce data using kinesis
                    (c) click on deploy

4- now go to S3 bucket
 	     (a) click on create bucket
     (b) select bucket type
  	     (c) give bucket name
  	     (d) untick the block public function
                     (e) tick on I acknowledge
                     (f) then click on create bucket

5- now create another lambda function
     (a) write a source code of consumer which will help to transfer the data from data  stream to consumer.
                    (b) deploy the code
   
6- now go to the first lambda function
    (a) Add the trigger
                    (b) select kinesis
                    (c) select your data stream
   (d) click on add

24.	RDS-
1 - go to AWS website and sign in to AWS console.
2 - then go to services
3 - click on RDS and you move to RDS dashboard
4 - then it shows resources such as DB resources and DB cluster
5 - you can click on create database and select region.
6 - then in configuration you can select database engine.
7 - then DB instance size is selected
8 - click on free tier.
9 - then provide master name, password.
10- review the settings.
11- click on create database.

25.	create VPC with IG gateway & create public & private subnets.
1 - sign in AWS console.
2 - navigate to VPC from services section.
3 - click on your VPC from navigation page & click create VPC
4 - Enter the details
      (a) name tag for VPC
      (b) IPV4
      (c) IPV6
      (d) Tenecy - choose default 
      (e) click on create VPC
5 - Create Internet gateway & attach VPC to it.
6 - Create subnets:-
      Private Subnets:
     (a) click on subnets in navigation panel on VPC dashboard
     (b) click on create subnet
     (c) Enter details: Name tag, select VPC, availability zone, IPV4
     (d) click on create subnet.



        Public subnet:-
      (a) click on subnets in navigation panel on VPC dashboard
      (b) click on create subnet
      (c) Enter details: Name tag, select VPC, availability zone, IPV4
      (d) click on create subnet.
      (e) enable auto assign public IP
      (f) configure route tables.

26.	Elastic BeanStalk
 a)In the search bar, search for Elastic BeanStalk.
 b)Click on Create Application.
 c)In Environment tier, click on web server environment.
 d)Give name to your envirornment.
 e)In platform type, choose managed platform.
 f)In platform, choose language in which you want create your application,(Node.js)
 g)In application code, click on sample application.
 h)In presets, choose single instance that is free tier eligible.
i)Click on next.
j)In service role, click on create and use new service role.
k)Choose any key pair.
l)Click on next.
m)Choose default VPC.
n)In Public IP address, check activated.
o)Click next.
p)Choose t2.micro as Instance Type.
q)Click next.
r)Click on submit.

27.	Kubernets
a)Log in to your AWS Account.
b) Navigate to the IAM Dashboard in the AWS Management Console.
c)Create a new user with administrative privileges and add the user to an "Administrators" group.
d) Follow the installation instructions provided at AWS CLI Installation.
e) Open a terminal and run the command:
     aws configure
f) Enter your AWS Access Key ID, Secret Access Key, region (e.g., us-west-2), and output format (e.g., json).
g) Follow the installation instructions available at Minikube Installation.
h) Follow the instructions at Install kubectl.
i)In the terminal run the command:-
minikube start --driver=virtualbox
k)Check the nodes in the cluster by running
kubectl get nodes
l) Deploy an NGINX container by executing
kubectl create deployment nginx --image=nginx
m)Retrieve the URL to access your deployment bu running
minikube service nginx –url
n)Launch the kubernet dashboard by executing
minikube dashboard
o)Stop the minikube cluster by running
minikube stop
p)Delete the minikube cluster by running:-
minikube stop

28.	Dynamo DB
a)Search for DynamoDB
b)Click on create Table.
c)Give table Name
d)Give name to the partition key. Partition Key must be Unique and not-null.
e)Give name to the sort key. Sort key is optional.
f)Keep the table settings as default.
g)Click on create table.
h)Now to add table items, Click on Explore table items.
i)Click on create items.
j)Here,enter the value of partition key and sort key.
k)To add more attributes to your table, click on add new attribute.
l)Choose the datatype of attribute.
m)Click on create item.
n)Now, if you want to perform any filter on you table , you can click on filter and enter attribute name , conditions and value and click on run.
o)After you click on run, you will get desired results of your applied filter.
---------------------------------------------------------------------------------------------------------------------------------------

1-	Elastic Compute Cloud (EC2)
2-	To access Windows EC2 instance
3-	To access Linux EC2 instance
4-	To install some software on Linux EC2 instance
5-	Amazon Machine Image
6-	Create Load Balancer
7-	S3 Bucket
8-	Virtual Private Cloud
       11-  Image building and uploading using Docker on Docker Hub
       14- CloudWatch (To stop Linux instance if CPU utilization is less than or equal to 15%)
       15- CloudFormation (using CloudFormation template for automatic creation of instance)
       16- AWS Lambda
       17- AWS Key Management Services
	18- Directory Services
	19- Amazon Lex (to create chat based services and take data from user)
	20- Creating custom variable slot in chatbot (giving menu to the user)
       21- Steps to launch windows instance using EC2
       22- Detailed steps to create an application load balancer in the AWS management console
       23- Kinesis: Use two lambda function (Producer and consumer) to generate data through Kinesis    and put lambda function in S3 bucket
       24- RDS
       25- create VPC with IG gateway & create public & private subnets.
       26- Elastic BeanStalk
       27- Kubernets
       28- Dynamo DB
