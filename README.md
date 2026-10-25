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

![CloudTrail MFA JSON Analysis](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/2d9f3dc97d0e13173755130dd84903cad5a0b7bb/Screenshot%202026-10-23%20134555.png)

*Figure 7: AWS CloudTrail CheckMfa JSON record confirming successful WebAuthn MFA verification.*

### Step 8 — Review Existing CloudTrail Configuration

Reviewed the AWS CloudTrail Trails dashboard to determine whether an audit logging trail was already configured.

Confirmed that no CloudTrail trails were displayed in the current AWS account and region.

Identified the need for a dedicated trail to support persistent audit logging, centralized log storage, and future security monitoring integrations.

![CloudTrail Trails Review](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/1797c541834403a586d5b732c9b0a49e44c523ab/Screenshot%202026-10-23%20134708.png)

*Figure 8: AWS CloudTrail Trails dashboard showing no existing trails before security logging configuration.*

### Step 9 — Create a Multi-Region CloudTrail Trail

Created a multi-region AWS CloudTrail trail named `CloudSecurity-Monitoring-Trail` to establish persistent logging of AWS management activity across regions.

Configured the trail to capture read and write management events, deliver log files to an Amazon S3 bucket, and enable log file validation to support integrity verification. Disabled optional features that were not required for the initial deployment, including CloudWatch Logs integration, SNS notifications, Insights events, and recursive logging.

The AWS CloudTrail console confirmed that the trail was successfully created.

![CloudTrail trail successfully created](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/2d9f3dc97d0e13173755130dd84903cad5a0b7bb/Screenshot%202026-10-23%20140249.png)

*Figure 9: Successfully created the multi-region CloudTrail trail with an Amazon S3 log destination.*
  
### Step 10 — Verify CloudTrail Logging Status

Opened the newly created `CloudSecurity-Monitoring-Trail` and verified that its logging status displayed **Logging**.

Confirmed that the trail is configured for multi-region monitoring with log file validation enabled. Reviewed the Amazon S3 log destination and verified that CloudWatch Logs and SNS notification delivery remain disabled pending later configuration.

At the time of verification, the console had not yet reported a log file delivery timestamp. S3 log delivery will be verified separately.

![CloudTrail logging status](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/e6b827cdc8862f35d36a25a93a90533b8aec9db6/Screenshot%202026-10-23%20140456.png)

*Figure 10: CloudTrail trail details showing active logging status and the configured S3 log destination.*
  
### Step 11 — Verify CloudTrail Log Delivery to Amazon S3

Navigated to the Amazon S3 bucket associated with `CloudSecurity-Monitoring-Trail` and inspected the regional CloudTrail log directory.

Located 33 compressed `.json.gz` log files in the `us-east-1/2026/10/23/` directory, confirming that CloudTrail successfully delivered management-event logs to Amazon S3.

Verified the log file naming convention, delivery timestamps, and S3 Standard storage class. The bucket also contains a `CloudTrail-Digest/` directory associated with log file validation.

![CloudTrail logs delivered to Amazon S3](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/bb957a7b36f7eda3e866fe05b0a1654c8981cbac/Screenshot%202026-10-23%20215143.png)

*Figure 11: Amazon S3 displaying 33 compressed CloudTrail log files delivered to the regional log directory.*
  
### Step 12 — Analyze CloudTrail JSON Event Logs

Downloaded a CloudTrail log file from Amazon S3 and used Windows PowerShell to parse its JSON records.

Extracted event timestamps, API operation names, AWS service sources, and regions to examine recorded management activity. Identified five events involving Amazon S3 bucket creation and policy configuration, CloudTrail Insights settings retrieval, and AWS STS role assumption.

Confirmed that the log contains structured AWS API activity records suitable for security investigations and auditing.

![CloudTrail JSON event analysis in PowerShell](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/9e1cf28730e9076204348ebae4f6bdaee42a6eb7/Screenshot%202026-10-23%20231026.png)

*Figure 12: PowerShell displaying CloudTrail API event timestamps, operation names, service sources, and AWS regions.*
  
### Step 13 — Investigate an AWS S3 API Event

Investigated a `CreateBucket` management event from an Amazon S3 CloudTrail log using Windows PowerShell.

Parsed the event's `userIdentity`, `sessionContext`, and `requestParameters` fields to identify the initiating IAM user, MFA authentication status, and affected S3 resource.

Confirmed that the `cloud-lab-admin` IAM user initiated the operation through an MFA-authenticated session. The target resource matched the S3 bucket created for CloudTrail log storage.

The findings were consistent with the authorized CloudTrail setup activity. No suspicious behavior was established from this event.

![CloudTrail S3 API event investigation](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/885b95b71b90383c0bebf13a34370a8f725592f9/Screenshot%202026-10-23%20234806.png)

*Figure 13: PowerShell investigation of an S3 CreateBucket event, identifying the IAM user, MFA status, and affected resource.*
  
### Step 14 — Access Amazon CloudWatch

Opened the Amazon CloudWatch console in the `us-east-1` region to begin configuring security monitoring for the AWS environment.

Reviewed the CloudWatch overview and identified the available logging, metrics, dashboards, and alarm capabilities. Confirmed that no CloudWatch alarms were configured at the time of the initial review.

This establishes the starting point for integrating CloudTrail management events with CloudWatch Logs and developing security monitoring alerts.

![Amazon CloudWatch console overview](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/1ce6b1fbbdc475a0af4a4e2b7e4b23600bb588ab/Screenshot%202026-10-23%20235609.png)

*Figure 14: Amazon CloudWatch overview before configuring log monitoring and security alerts.*
  

### Step 15 — Review CloudWatch Log Groups

Opened Amazon CloudWatch Log Management in the `us-east-1` region to inspect existing log groups before configuring CloudTrail integration.

Confirmed that no CloudWatch log groups were present. This established a baseline for creating a dedicated security monitoring log group and configuring log retention.

![CloudWatch log groups review](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/6bd09a31d924113a9c5912b2738de707b0b75931/Screenshot%202026-10-24%20140213.png)

*Figure 15: CloudWatch Log Management showing zero existing log groups before CloudTrail integration.*

### Step 16 — Create a CloudWatch Security Monitoring Log Group

Created a dedicated Amazon CloudWatch Logs group named `/aws/cloudtrail/CloudSecurity-Monitoring` to support centralized monitoring of AWS CloudTrail management events.

Configured the Standard log class to support security metric filters and established a seven-day retention policy to help manage log storage costs.

Verified that the log group was successfully created with deletion protection disabled and no metric filters configured.

![CloudWatch security log group](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/4a3d481de1763a78a9d2b11cccf0578e901e11d0/Screenshot%202026-10-24%20142415.png)

*Figure 16: CloudWatch security monitoring log group successfully created with Standard log class and seven-day retention.*

### Step 17 — Integrate AWS CloudTrail with CloudWatch Logs

Configured the existing `CloudSecurity-Monitoring-Trail` to deliver AWS management events to the CloudWatch log group `/aws/cloudtrail/CloudSecurity-Monitoring`.

Created a dedicated IAM service role named `CloudSecurity-CloudTrail-CloudWatch-Role` with permissions to create CloudWatch log streams and publish log events.

Verified that the CloudTrail configuration displays the associated CloudWatch log group and IAM role while S3 log delivery and trail logging remain active.

![CloudTrail CloudWatch integration](https://github.com/Kevinolee1/AWS-CloudTrail-CloudWatch-Security-Monitoring/blob/b2f11dc6248eb1d0ea1f3970e495a7a4b23d0b4e/Screenshot%202026-10-24%20144246.png)

*Figure 17: CloudTrail successfully configured with a dedicated CloudWatch Logs group and IAM service role.*

### Step 18 — Verify CloudTrail Log Delivery in CloudWatch

Opened the CloudWatch log group `/aws/cloudtrail/CloudSecurity-Monitoring` and verified that three CloudTrail log streams were present.

Inspected a log stream containing 38 loaded events and reviewed JSON records for AWS API activity, IAM identity, MFA authentication, event sources, timestamps, and management operations.

Identified read-only API activity, including `DescribeLogGroups`, `DescribeAlarms`, and `DescribeRegions`, confirming that CloudTrail management events were successfully delivered to CloudWatch Logs.

![CloudWatch CloudTrail log events](screenshots/18-cloudwatch-cloudtrail-log-events.png)

*Figure 18: CloudWatch Logs displaying CloudTrail management events with IAM identity and API activity details.*

### Step 19 — Configure Failed Console Login Detection

Created a CloudWatch Logs metric filter named `CloudSecurity-FailedConsoleLogin-Filter` to identify unsuccessful AWS Management Console authentication attempts.

Configured the filter to inspect CloudTrail JSON events for `ConsoleLogin` activity with a `Failure` response.

Assigned matching events to the custom CloudWatch metric `CloudSecurity/Authentication / FailedConsoleLoginCount`, using a metric value of `1`, a default value of `0`, and the `Count` unit.

Verified that the metric filter was successfully created and associated with the CloudTrail monitoring log group.

![CloudWatch failed login metric filter](screenshots/19-cloudwatch-failed-login-metric-filter.png)

*Figure 19: CloudWatch metric filter configured to detect and count failed AWS Console login attempts.*

### Step 20 — Configure Failed Console Login Security Alarm

Created a CloudWatch metric alarm named `CloudSecurity-FailedConsoleLogin-Alarm` to monitor unsuccessful AWS Management Console authentication attempts.

Configured the alarm to monitor the custom `FailedConsoleLoginCount` metric using the Sum statistic and a five-minute evaluation period. The alarm threshold is set to one or more failed login attempts within a single evaluation period.

Integrated the alarm with the `CloudSecurity-FailedLogin-Alerts` Amazon SNS topic and confirmed the email subscription to enable security notifications.

Verified that the alarm was successfully created with actions enabled. The initial state was `Insufficient data` because the custom metric had not yet produced sufficient datapoints.

![CloudWatch failed login security alarm](screenshots/20-cloudwatch-failed-login-security-alarm.png)

*Figure 20: CloudWatch security alarm monitoring failed AWS Console logins with an SNS notification action.*
