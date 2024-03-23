## 🔎 New Hire Old Artifacts
Follow along: https://tryhackme.com/r/room/newhireoldartifacts

We will be going through the logs on splunk and answering the questions.

## Question 1: A Web Browser Password Viewer executed on the infected machine. What is the name of the binary? Enter the full path.

First, change the time period to December 2021, so we only get logs for that time period.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/8d67c365-1992-47e1-9d76-7094bca5ed27)

Next, we are supposed to find the "Web Browser Password Viewer", so we narrow down our search with the word "password".

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/c13d1af9-2c10-4ad1-a47e-9c41de1b9b62)

We see the "Web Browser Password Viewer" in the description.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/c5ea8c9b-f5e2-43ec-b228-be25c48d0808)

Let's narrow down the search even more and add "Web Browser Password Viewer.".

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/feef4c09-d4f2-49e6-91bd-54b3b762a0f4)

After extending the first windows event log seen, we see the image showing the path of the binary.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/5f07b903-c8c9-48fc-a1e4-74e742236d78)

Answer: C:\Users\FINANC~1\AppData\Local\Temp\11111.exe

## Question 2: What is listed as the company name?

We can see the company name by clicking on "Company" under the interesting fields section.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/16cf1fe4-55b1-4793-b0fd-2a2160c0c89d)

Answer: NirSoft

## Question 3: Another suspicious binary running from the same folder was executed on the workstation. What was the name of the binary? What is listed as its original filename? (format: file.xyz,file.xyz)

Narrow down the search by searching for logs coming from the infected system (someone in finance).

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/23c40df2-1a08-4c0c-a936-f0677c9cd4c8)

The question mentions that the binary is ran from the same folder. Looking at the image paths, we see another binary being downloaded from the same folder (Temp). 

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/94a58344-9eb0-40a7-af3a-5035fe5ec6f3)

We have found the name of the binary now lets find out its original filename. Search for "OriginalFileName" in the fields and select it. (if it's not visible already)

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/de70d45c-d255-4527-96e1-39a4d4bf1893)

We can see two original file names. Because of the format provided, PlaitExplorer.exe is the right answer.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/3fe297b6-c909-4ea5-a5da-9eecf0ec30d5)

Answer: IonicLarge.exe, PalitExplorer.exe

## Question 4: The binary from the previous question made two outbound connections to a malicious IP address. What was the IP address? Enter the answer in a defang format.

Search for the binary and select the "DestinationIp" field.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/b7c17f81-f7e5-4b64-90ef-581b78acc987)

Two outbound connections can be seen to the IP "2.56.59.42". The answer must be in defang format so use cyberchef to defang the IP.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/92d787e6-8e76-489d-9574-c19f76aa46ed)

Answer: 2[.]56[.]59[.]42

## Question 5: The same binary made some change to a registry key. What was the key path?

Doing a quick search on google, we can see that the Event ID 13 is logged when a resgitry key is modified.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/0f708718-921e-4b35-9e34-7266d047e9bc)

Click on the event code 13 and observe the key path. We can see the same key path being used. The path is "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender"

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/f389a088-1073-4da1-b6c1-3d8c416eb4ea)

Answer: HKLM\SOFTWARE\Policies\Microsoft\Windows Defender

## Question 6: Some processes were killed and the associated binaries were deleted. What were the names of the two binaries? (format: file.xyz,file.xyz)

The hint we are given is "Process were killed with 'taskkill /im'". So search for "taskkill /im".

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/a9ba928d-76d8-431e-b689-f1e7d5c25763)

Going through the logs, we see the two binaries we need, "phcIAmLJMAIMSa9j9MpgJo1m.exe" and "WvmIOrcfsuILdX6SNwIRmGOJ.exe".

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/5e0e503b-5fd5-4f76-8d59-59cfe5bfcbe3)

Answer: phcIAmLJMAIMSa9j9MpgJo1m.exe,WvmIOrcfsuILdX6SNwIRmGOJ.exe

## Question 7: The attacker ran several commands within a PowerShell session to change the behaviour of Windows Defender. What was the last command executed in the series of similar commands?

Make a search using the keywords mentioned in the question. 

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/c9c41675-5455-45ed-8b59-fe03faff5ece)

The first powershell command we see is our answer. (The command starts with "powershell...")

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/77740f88-a349-4d5c-927e-b00d706d33b9)

Answer: powershell  WMIC /NAMESPACE:\\root\Microsoft\Windows\Defender PATH MSFT_MpPreference call Add ThreatIDDefaultAction_Ids=2147737394 ThreatIDDefaultAction_Actions=6 Force=True

## Question 8: Based on the previous answer, what were the four IDs set by the attacker? Enter the answer in order of execution. (format: 1st,2nd,3rd,4th)

Based on the previous question, they are talking about the "ThreatIDDefaultAction_Ids". Select the "ParentCommandLine" field (we found the powershell command here).
The second value shows all the 4 IDs set with them being executed in order.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/29edf4af-0738-42b2-8ec9-527834fae1aa)

Answer: 2147735503,2147737010,2147737007,2147737394

## Question 9: Another malicious binary was executed on the infected workstation from another AppData location. What was the full path to the binary?

Search for AppData and select the image field to see all the file paths.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/a5d1d4b2-76df-45ea-9f27-34336735e262)

We need to go through the binaries we havne't investigated yet and see which one is malicious. The "EasyCalc.exe" is a malicious file and is being hidden under the name of a legitemate app.
It does not have a description which the actual app would have.

Answer: C:\Users\Finance01\AppData\Roaming\EasyCalc\EasyCalc.exe

## Question 10: What were the DLLs that were loaded from the binary from the previous question? Enter the answers in alphabetical order. (format: file1.dll,file2.dll,file3.dll)

Search for the keyword "dll" with the previous path as the image field.

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/377efc7c-69b4-46d0-89d6-1c82ea243401)

Select the "ImageLoaded" field to see all the dll files

![image](https://github.com/moromerx/SOC-Tier-1/assets/162036545/4f71886c-a65e-43c9-9644-a5342ecdaaef)

Enter them in alphabetical order.
Answer: ffmpeg.dll,nw.dll,nw_elf.dll

Done! 🎉
