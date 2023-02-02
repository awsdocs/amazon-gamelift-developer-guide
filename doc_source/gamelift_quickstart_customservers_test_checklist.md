# Testing<a name="gamelift_quickstart_customservers_test_checklist"></a>

Use the following checklist to track testing items while developing your game with GameLift hosting\. Items marked **\[Critical\]** are critical for your production launch\.
+ **\[Critical\]** Complete the launch questionnaire, and submit the completed questionnaire to the GameLift launch team\. You can find the launch questionnaire in the [GameLift console](https://console.aws.amazon.com/gamelift/prepare-to-launch)\.
+ **\[Critical\]** [Request increases for GameLift service quotas](limits-regions.md) and other AWS service quotas so that your live environment can scale up to production needs\.
+ **\[Critical\]** Verify that the open ports on live fleets match the range of ports that your servers could use\.
+ **\[Critical\]** Close RDP port 3389 and SSH port 22\.
+ Develop a plan for the DevOps management of your game\. If you're using Amazon CloudWatch Logs or Amazon CloudWatch custom metrics, define alarms for severe or critical problems on the server fleet\. Simulate failures and test the runbooks\.
+ [Verify that the number of servers](gamelift-compute.md) running on an instance at full usage are within the capabilities of the server instance type\.
+ [Tune your scaling policy](fleets-manage-capacity.md) to be more conservative at first and provide more idle capacity than you think you need\. You can optimize for cost later\. Consider the use of target\-based scaling policy with 20 percent idle capacity\.
+ [Use FlexMatch latency rules](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html) to match players who are geographically near the same AWS Region\. Test how this behaves under load with synthetic latency data from your load test client\.
+ Load test your player authentication and game session infrastructure to see if it scales effectively to meet demand\.
+ Verify that a server left running for several days can still accept connections\.
+ Raise your AWS Support plan level to Business or Enterprise so that AWS can respond to you during problems or outages\.