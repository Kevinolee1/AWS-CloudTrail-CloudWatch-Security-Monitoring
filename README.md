# AWS-CloudTrail-CloudWatch-Security-Monitoring
Hands-on AWS cloud security project focused on monitoring account activity, analyzing security events, detecting suspicious behavior, and configuring automated alerts using CloudTrail, CloudWatch, EventBridge, and SNS.


### Step 1 — Access AWS CloudTrail

Opened the AWS CloudTrail console in the US East (N. Virginia) region using an IAM administrative account.

CloudTrail provides auditing capabilities for AWS account activity, including API operations, identity activity, and resource management events.

This step establishes the starting point for configuring AWS security monitoring and investigating account activity.

![AWS CloudTrail Console](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/62b3771255f3e1b569932e542f62929a8064fffd/Screenshot%202026-10-23%20013332.png)

*Figure 1: AWS CloudTrail console opened to begin the security monitoring lab.*

### Step 2 — Review AWS CloudTrail Event History

Accessed AWS CloudTrail Event History to examine management events recorded during the previous 90 days.

Reviewed IAM sign-in activity, MFA checks, EC2 instance modifications, and instance termination events. Applied a read-only filter to focus on management actions that modify AWS resources.

Verified that CloudTrail records event names, timestamps, user identities, event sources, and affected resources for security auditing and incident investigations.

![CloudTrail Event History](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/c9188a1d667e60fae339c3dd2e9018b18a78f324/Screenshot%202026-10-23%20014446.png)

*Figure 2: AWS CloudTrail Event History displaying account activity and administrative API events.*

### Step 3A — Investigate AWS Console Login Activity

Investigated a ConsoleLogin event in AWS CloudTrail to examine IAM authentication activity.

Reviewed the IAM username, event timestamp, source IP address, AWS Region, event source, and unique event ID.

Identified how CloudTrail provides authentication records that support security auditing, suspicious login investigations, and incident response.

The login result and MFA status require further verification using the event's JSON record.

![CloudTrail Console Login Event](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/3021e1367811decfa9aee2a5fb944766d4a16701/Screenshot%202026-10-23%20014710.png)

*Figure 3: AWS CloudTrail ConsoleLogin event displaying IAM authentication details and source IP information.*

### Step 3B — Investigate AWS Console Login Activity

Investigated an IAM ConsoleLogin event using AWS CloudTrail Event History and its JSON event record.

Verified that the authentication was successful and multi-factor authentication (MFA) was used. Reviewed the IAM identity, event timestamp, source IP address, AWS Region, and event metadata.

Confirmed that the event was categorized as a management event and that the connection used TLS 1.3.

### Step 4 — Investigate EC2 Instance Termination

Investigated a TerminateInstances management event using AWS CloudTrail Event History.

Reviewed the IAM identity, event timestamp, source IP address, AWS Region, and affected EC2 instance.

Verified that CloudTrail recorded the administrative API request and associated resource, demonstrating how AWS audit logs support infrastructure change tracking, accountability, and security investigations.

![CloudTrail EC2 Termination Event](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/2c86d938a79e1cc655644326aa7688c9aaa52e84/Screenshot%202026-10-23%20015655.png)

*Figure 4: AWS CloudTrail TerminateInstances event showing the IAM identity and affected EC2 resource.*

### Step 5 — Analyze EC2 Termination Event JSON

Analyzed the CloudTrail TerminateInstances JSON record to investigate an AWS infrastructure change.

Verified that the API request originated from an MFA-authenticated IAM session and targeted the temporary EC2 instance created during the previous VPC security lab.

Examined the request and response elements to confirm that the instance transitioned from stopped to terminated without forced termination.

Demonstrated how CloudTrail JSON records support security investigations, administrative accountability, and verification of infrastructure changes.

![EC2 Termination JSON Analysis](screenshots/05-cloudtrail-termination-json-analysis.png)

*Figure 5: AWS CloudTrail JSON event confirming an MFA-authenticated EC2 termination and the resulting instance state.*

Demonstrated how CloudTrail authentication logs support identity monitoring, security auditing, and incident investigations.

![CloudTrail Console Login Event](screenshots/03-cloudtrail-console-login-event.png)

*Figure 3: AWS CloudTrail ConsoleLogin event showing authentication details, successful login, and MFA verification.*
