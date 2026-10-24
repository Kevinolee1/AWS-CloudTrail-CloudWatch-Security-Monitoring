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

*Figure 3A: AWS CloudTrail ConsoleLogin event displaying IAM authentication details and source IP information.*

### Step 3B — Investigate AWS Console Login Activity

Investigated an IAM ConsoleLogin event using AWS CloudTrail Event History and its JSON event record.

Verified that the authentication was successful and multi-factor authentication (MFA) was used. Reviewed the IAM identity, event timestamp, source IP address, AWS Region, and event metadata.

Confirmed that the event was categorized as a management event and that the connection used TLS 1.3.

![CloudTrail Console Login Event](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/2c86d938a79e1cc655644326aa7688c9aaa52e84/Screenshot%202026-10-23%20015655.pn)

*Figure 3B: AWS CloudTrail ConsoleLogin event showing authentication details, successful login, and MFA verification.*

### Step 4 — Investigate EC2 Instance Termination

Investigated a TerminateInstances management event using AWS CloudTrail Event History.

Reviewed the IAM identity, event timestamp, source IP address, AWS Region, and affected EC2 instance.

Verified that CloudTrail recorded the administrative API request and associated resource, demonstrating how AWS audit logs support infrastructure change tracking, accountability, and security investigations.

![CloudTrail EC2 Termination Event](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/193519860b1843451151a47c4067e02509ed3706/Screenshot%202026-10-23%20015923.png)

*Figure 4: AWS CloudTrail TerminateInstances event showing the IAM identity and affected EC2 resource.*

### Step 5 — Analyze EC2 Termination Event JSON

Analyzed the CloudTrail TerminateInstances JSON record to investigate an AWS infrastructure change.

Verified that the API request originated from an MFA-authenticated IAM session and targeted the temporary EC2 instance created during the previous VPC security lab.

Examined the request and response elements to confirm that the instance transitioned from stopped to terminated without forced termination.

Demonstrated how CloudTrail JSON records support security investigations, administrative accountability, and verification of infrastructure changes.

![EC2 Termination JSON Analysis](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/6fac479581d457ce82d41175c9ec8298398c68e6/Screenshot%202026-10-23%20020219.png)

*Figure 5: AWS CloudTrail JSON event confirming an MFA-authenticated EC2 termination and the resulting instance state.*

Demonstrated how CloudTrail authentication logs support identity monitoring, security auditing, and incident investigations.


### Step 6 — Filter CloudTrail MFA Authentication Events

Used AWS CloudTrail Event History to investigate MFA-related authentication activity by applying an Event name filter for CheckMfa.

Identified seven matching events associated with two IAM identities, demonstrating how CloudTrail records can be filtered to examine authentication activity across AWS users.

Established a starting point for investigating MFA verification events and identifying potential authentication anomalies.

![CloudTrail MFA Event Filter](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/3eb6d2f60a905788661a7699cece57b6ac43e2c0/Screenshot%202026-10-23%20134116.png)

*Figure 6: AWS CloudTrail Event History displaying seven CheckMfa events across two IAM identities.*

### Step 7 — Investigate MFA Authentication Event

Investigated an individual CheckMfa event in AWS CloudTrail to verify the result of an IAM multi-factor authentication attempt.

Analyzed the JSON event record and confirmed that MFA verification succeeded using WebAuthn MFA.

Reviewed the IAM identity, authentication timestamp, event source, event category, and TLS security details.

Demonstrated how CloudTrail authentication logs support identity security monitoring, MFA verification, and incident investigations.

![CloudTrail MFA JSON Analysis](screenshots/07-cloudtrail-mfa-json-analysis.png)

*Figure 7: AWS CloudTrail CheckMfa JSON record confirming successful WebAuthn MFA verification.*

### Step 8 — Review Existing CloudTrail Configuration

Reviewed the AWS CloudTrail Trails dashboard to determine whether an audit logging trail was already configured.

Confirmed that no CloudTrail trails were displayed in the current AWS account and region.

Identified the need for a dedicated trail to support persistent audit logging, centralized log storage, and future security monitoring integrations.

![CloudTrail Trails Review](screenshots/08-cloudtrail-existing-trails-review.png)

*Figure 8: AWS CloudTrail Trails dashboard showing no existing trails before security logging configuration.*

### Step 9 — Create a Multi-Region CloudTrail Trail

Created a multi-region AWS CloudTrail trail named `CloudSecurity-Monitoring-Trail` to establish persistent logging of AWS management activity across regions.

Configured the trail to capture read and write management events, deliver log files to an Amazon S3 bucket, and enable log file validation to support integrity verification. Disabled optional features that were not required for the initial deployment, including CloudWatch Logs integration, SNS notifications, Insights events, and recursive logging.

The AWS CloudTrail console confirmed that the trail was successfully created.

![CloudTrail trail successfully created](screenshots/09-cloudtrail-trail-created.png)

*Figure 9: Successfully created the multi-region CloudTrail trail with an Amazon S3 log destination.*
  
### Step 10 — Verify CloudTrail Logging Status

Opened the newly created `CloudSecurity-Monitoring-Trail` and verified that its logging status displayed **Logging**.

Confirmed that the trail is configured for multi-region monitoring with log file validation enabled. Reviewed the Amazon S3 log destination and verified that CloudWatch Logs and SNS notification delivery remain disabled pending later configuration.

At the time of verification, the console had not yet reported a log file delivery timestamp. S3 log delivery will be verified separately.

![CloudTrail logging status](screenshots/10-cloudtrail-logging-status.png)

*Figure 10: CloudTrail trail details showing active logging status and the configured S3 log destination.*
  
