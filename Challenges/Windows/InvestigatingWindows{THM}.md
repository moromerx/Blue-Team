## Investigating Windows - THM

## Question 1: Whats the version and year of the Windows machine?

Type "systeminfo" in cmd to see system information. Alternatively, you can see the "About" section in the seetings (many ways to get this info).

![image](https://github.com/moromerx/Blue-Team/assets/162036545/82571685-9311-4540-875e-a72bbe769bc2)

Answer: Windows Server 2016

## Question 2: Which user logged in last?

Use the Windows Event Viewer, go to Windows Logs > Security, and filter the current logs with the event ID "4624".

![image](https://github.com/moromerx/Blue-Team/assets/162036545/73ebe254-a345-450b-a4bb-42a993a2e116)

![image](https://github.com/moromerx/Blue-Team/assets/162036545/a8838657-c1fb-497e-82df-a2cd29fce2bb)

The first few logs are logged by Windows, as it shows the account name as "SYSTEM". Then we see a log with the account name "Administrator".

![image](https://github.com/moromerx/Blue-Team/assets/162036545/3de4fe61-c2cc-4e5f-a588-8253bea25633)

Answer: Administrator

## Question 3: 

Open the cmd again and enter "net user John" and it will provide us with the last successful login date for John.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/21e8d36b-40ff-4269-adca-cfc13bc9b179)

![image](https://github.com/moromerx/Blue-Team/assets/162036545/7982e80b-0ce7-4f4e-ad60-93a6aabda068)

Answer: 03/02/2019 5:48:32 PM
