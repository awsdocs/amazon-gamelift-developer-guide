# Testing<a name="gamelift_quickstart_customservers_test_checklist"></a>

Use the following checklists to keep track of testing items\. Items marked **\[Critical\]** are critical for your production launch\. 
+ **\[Critical\]** Fill out the launch questionnaire\. You can find the launch questionnaire in the GameLift console\.
+ **\[Critical\]** Scale GameLift and AWS service quotas so your live environment can scale up to production needs\.
+ **\[Critical\]** Verify that the ports that are open on live fleets match the range of ports that your servers could use\.
+ **\[Critical\]** Close RDP port 3389 and SSH port 22\.
+ Develop a plan for DevOps management of you game\. If you are using CloudWatch Logs or CloudWatch custom metrics, define alarms for severe or fatal problems on the server fleet\. Simulate failures and test the runbooks\.
+ Verify that the number of servers running on an instance at full usage are within the capabilities of the server instance type\.
+ Tune your scaling policy to be more conservative at first, provide more idle capacity than you think you need\. You can optimize for cost later\. Consider the use of target\-based scaling policy with 20% idle capacity\.
+ Use FlexMatch latency rules so that players who are logically close to the same Region play together\. Test how this behaves under load with synthetic latency data from your load test client\.
+ Load test your player authentication and game session infrastructure can scale effectively to meet demand\.
+ Verify that a server left running for several days is still able to accept connections\.
+ Raise your AWS Support level to business or enterprise level so AWS can respond to you during problems or outages\.