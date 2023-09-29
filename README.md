OBSERVABILITY OF URL SHORTENER FUNCTIONALITY 

PURPOSE
__________

"The Story: Nike emailed you about the URL Shortener. In Nike's email, Nike mentioned customers are complaining about the amount of time it takes to load their webpages. Nike will like us to reduce the amount of latency customers are experiencing." As an SRE engineer, I need to test the functionality of the url shortener associated with the web application. For Nike's standards and business needs, it isn't enough for the web application to just function. The application needs to be continuously optimized and tested to increase functionality as user traffic increases to the web page server. In order to truly measure the speed at which users can access the web application, I need to use a tool to meassure the latency before and after our chosen solution with multiple tests to ensure latency is reduced. Overall, I need to improve the functionality of the services that the URL shortener for the Nike web application provides for Nike and its users. 


LATENCY MEASUREMENT TESTING: 
________________

Fill out Blitz Creation Form: 

- Test Plan Section: Choose 'basic traffic increase' under **Blitz package**
- Thread group section: Select **1000** for **number of threads (users)** (*simulated user traffic based on if users tried to access the url shortener at the same time to see if the web         application can handle it*)
- Ramp-up period(seconds): 2
- Loop Count: 1
- Request Configuration Section: Choose *HTTP* protocol
- Server name or IP address: <url shortener>
- Port number: 80 to make sure that you access the correct web application 
- HTTP request: GET to test the retrieval of information from the site 

Using the same region and availability zone as where the web application was deployed from, I ran the first latency test in the same region and availabity zone. It took the test server 40.879 ms to access the web application. This amount of latency is too high and does not meet Nike's standards because its causing a negative user experience according to their customer feedback. We want something to capture the traffic so I created a CDN(content delivery/distribution network) through AWS Cloudfront to establish interconnected servers across availability zones to reduce the distance between users and the webpage server.


CREATING A CDN IN FRONT OF THE URL SHORTENER
______________________________________

1. Go to CloudFront on AWS

2. Create distributuion

3. Open elastic beanstalk where app is deployed --> copy domain *remove 'backslash at the end (\)*

4. Leave at HTTP only 

5. Port 80 

Scroll down to Cache Key and origin requests:
6. Clik **cache policy** --> select **cache optimized** *(to cache static content)*

Scroll down to web applicaiton firewall (WAF)
7. Do not enable security protections *(A web applicaton firewall was not necessary for this test. In the future, I would want to enable this service to  protect my applications from miscellaneous activity that may negatively affect the application's functionality)*

8. Use edge location under settings = use all edge locations *(suggested best choice for performance because this service comes with established edge locations of servers and multiple back up server locations. In case one data center location of servers goes down, there are other locations that can take the web traffic. These locations are also cached through regional edge caches to store the regional locations of data centers.)*

9. Select **Create distribution**

10. Go back to list of distributions --> copy distribution domain name --> paste URL in another browser. [The web application was served up but I can't measure the latency through this without testing the web traffic when accessing the web application across the internet]

11. Take **distribution domain name** to run another on our tool to measure the latency experienced when accessing the web application 


After establishing a CDN to store the images and videos on the webpage of the application, I ran another test that showed the amount of latency for the test server to reach the web application reduced to 9.652 ms. The CDN helps further reduce latency by caching or temporarily storing the static content of the application which includes the web page so that users can access the content over the internet quicker. I reduced the communication across the internet for the users to reach the web page because the servers have less content to deliver with the CDN's getting accessed before it, which ultimately decreases the web traffic to the server. Knowing where users are located helps SRE engineers know how to measure the amount latency that users experience. 

![Before & After CDN](https://github.com/DANNYDEE93/Latency_Test/blob/main/blitz1.jpg)
 


ALTERNATIVE WAYS TO CONTROL WEB TRAFFIC 
________________________________________
-Create a new EC2 instance and load balancers to split up the traffic amongst multiple servers.
-Ping the application with multiple get requests to test the durability of the server to handle increased web traffic.
 Check how long it takes for packets to be retrieved to measure latency
-Increase availability zones to balance out the traffic if overwhelmed with requests.
