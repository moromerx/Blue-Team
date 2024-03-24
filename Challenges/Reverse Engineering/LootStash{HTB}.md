## 💰 Loot Stash - HTB (Very Easy)

Follow along: [rev_lootstash.zip](https://github.com/moromerx/Blue-Team/blob/main/Challenges/Reverse%20Engineering/Files/rev_lootstash.zip)

Unzip the file

![image](https://github.com/moromerx/Blue-Team/assets/162036545/3299b17f-d07f-4a15-9928-22f525fbe024)

We get the following after running the file.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/c38189d9-dafa-4280-a7fd-7300f8d09dfb)

Run the strings command to see all the human readable text in the binary. You will see alot of strings.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/12513286-d72f-4ce4-b7ba-efa4dd79f811)

Run the strings command again but this time look for "HTB" in the strings and you will get the flag.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/eea55d92-1a75-409d-bb24-a0c244be26fe)

FLAG: HTB{n33dl3_1n_a_l00t_stack}

Done! 🎉
