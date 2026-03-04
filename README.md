# aws_notes

VPC(Virtual Private Cloud):- (Video:- https://www.youtube.com/watch?v=P8g7Z4NYk3Q)

what is VPC


what is VPC compare it to a physical data ceneter
why do we need VPC? we were actually doing deployments on EC2 instances without any issues on cloud then what is the need for VPC?
explain the village and construction example.

1. To define the size of VPC whenever a company/customer requests we will do it using IPaddress range. 10.0.0.0/24. similar to size of land
2. Internally for the same project n for different teams we can split the IP addresses, this concept is called subnet. similar to size of land allocated to each house.
3. To enter the subnets or the secured community we will need a gateway basically a security check to let u inside the community.
4. There are 2 subnets, private subnet not accessible to anyone from outside world, public subnet if someone has access to VPC then they can access public subnet.
5. Public subnet can be accessed through internet gateway.
6. If a user wants to access an application that is running inside a instance of the private subnet they cannot do it directly, the request will first go to internet gateway then to the publci subnet and then to the load balancer then via routing table rules defined in it then it goes to the EC2 instances inside the private subnet, again this does not guarantee on fulfulling the user request because there will be Security groups that will block the access.
7. If u want the resources in Private subnet to acess/download packages from internet we will use NAT gateway with this help we can mask the IP addresses of our private subnet resources to either route tables or load blanacer IP address.

8. Security Group:- Security at resource level
9. Security group has inbound traffic and outbound traffic rules
10. Inbound traffic:- a user trying to access the application running inside the EC2 instances
11. Outbound traffic:- the application inside the EC2 tries to access some google.com from internet.
12. whenever you create a EC2 instance it will be associated with the VPC in the same region as ur EC2 and it will create a SG for ur EC2 instance by default, all Outbound traffic will be allowed by default except for port25 and all inbound traffic is blocked.
13. NACL(Network access Control List):- security at subnet level
14. Even if the security is compromised at instance level by the developers maybe they would have given allow all traffic, we can still control the security at subnet level using NACL
15. Using NACL at subnet level we can specify what kind of traffic we want to deny and what we want to allow, whereas SG is only for allowing traffic.
16. NACL has priority over SG, less number rule has high priority
17. VPC endpoints:- If a node in private subnet wants to cumminicate with AWS services so instead of letting it communicate through the internet we will create endpoints.

    Practical:-
    1. whenever you create  a VPC, AWS will create a by default, NACL(all allow and all deny) and route table, n SG.
    2. Internet gateway is visible in the network connections section.
    3. I have created a EC2 instance with default SG and SSH access in the custom VPC and deployed an application on port 8000, when i try to access the application with the EC2 instance it will not be visible because in default SG in the inbound rules only SSH is allowed(allows us to connect to EC2).
   
Ur project references:-

In our project we have created, Security Group with inbound rules source as other security group because IP addresses are not static and the nodes has to communicate with each other and control plane in EKS cluster. these security groups are automatically created and have sefl referencing rules.

NOTE:- whenever a VPC is created a self referencing SG is created, whenever a EKS cluster is created or a node group is created a self referencing SG is created.

Important notes for EKS & VPC:-

1. If there is any incoming traffic to any resources in the public subnet like load balancers they must have the inbound rules assigned on that particular port.
2. If a node on which a pod is assigned has to pull the ECR image then it must have IAM role with neccessary permissions so that the ECR will let the node pull the image.
3. If a node on which the pod is asigned is in private subnet, then u will get "imgpullbackoff" error because the node does not have access to internet so it cannot pull the image from ECR.
4. To let the nodes in the private subnet communicate with other AWS services without using internet we will have to implement VPC endpoints.

There is a project for this refer AV videos.

AWS Lambda:-

AWS Lambda is a serverless compute service from Amazon Web Services that lets you run code without provisioning or managing servers.
AWS will take care of infrastructure, scaling and load balanacing and availabilty.
It will charge only for execution time.
You just upload your code, and Lambda runs it automatically in response to events.
⸻
🔹 How It Works (Simple Explanation)
1. You write code (Node.js, Python, .NET, Java, etc.).
2. You upload it to Lambda.
3. An event triggers it (like an API call, file upload, or message in a queue).
4. Lambda automatically:
• Allocates compute
• Runs your function
• Scales up/down
. It manages avaialabilty in multiple AZ by default.
• Charges only for execution time
You don’t manage EC2, OS, scaling, or patching.

🔹 What Actually Happens Behind the Scenes?
When a request comes:
1. Lambda spins up a lightweight runtime environment.
2. Your function runs.
3. After execution:
• If no more traffic → resources are released.
• If more traffic → Lambda creates multiple parallel environments automatically.
This is called serverless, but servers still exist — you just don’t manage them.

Using Lambda
• Resize product images on upload
• Send email after order placed
• Generate invoice PDFUsing Lambda

Lambda Video:- https://www.youtube.com/watch?v=XFGSuj83wdc&t=1262s

NOTE:- when creating Lambda you just specify which framework you want to use eg:- python nodejs, you dont specify anything else.
1. When you are using lambda, creating lambda_handler is very important.
2. There are multiple ways you can do code deployment to Lambda.
3. way1:- Directly in the AWS lambda UI itself you can create your code with handler event.
4. way2:- U have to create a zip file(eg:- if it is for python zip all the necessary libraries by installing them using pip) and upload it in the Lambda funtion, make sure the handler has the .py name and the handler function name same as ur code.
5. way3:- you can upload ur zip file from s3 buckets.
6. Lambda function URL:- if you want the lambda to be accessible via URL you have to enable this. either with public access or IAM access.
7. Environment Variable:- you can setup environment variables for ur lambda.
8. Layers(Library):- if you want same code to be deployed on multiple lambda then use this concept

Note:-
whenever you want to deploy code to AWS lambda using its console then you cannot install any external libraries like "requests" because AWS does not run any pip install for u. Few codes run without any issue in Lambda because it comes with some pre-installed libraries when we create lambda for python runtime.
So, if u want to make use of external libraries the u should deploy ur code via zip, because whenever AWS finds "import requests" it will search in the zipped folder if it is present or not.
