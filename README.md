# aws_notes

VPC(Virtual Private Cloud):- (Video:- https://www.youtube.com/watch?v=P8g7Z4NYk3Q)

what is VPC, 


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

8. 
