## 📦 PackedAway (Very Easy) - HTB
Follow along: [rev_packedaway.zip](https://github.com/moromerx/BlueTeam/blob/main/Challenges/Reverse%20Engineering/Files/rev_packedaway.zip)

Unzip the file.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/24c8c5fe-c4c6-4107-8813-71691217a4ca)

See which kind of binary it is using the "file" command. It shows that the file is an ELF (Executable and Linkable Format) binary, which is a common file format for executables on Unix-like systems. It is 64-bit, which means it's designed for a 64-bit architecture. LSB stands for "Least Significant Byte", which indicates the endianness of the binary (the byte order in which bytes are read). "pie" indicates that it is a Position Independent Executable, meaning it can be loaded at any address in memory without modification.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/7adb59d6-3b29-4b30-8c80-d0fbd3cbc92d)

Use the strings command to see all the readable content in the file.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/f459e27e-0bd7-49be-a11b-065b57a83d59)

Going through the strings, we see "This file is packed with the UPX executable packer".

![image](https://github.com/moromerx/Blue-Team/assets/162036545/d7fce28f-8d29-4774-a58a-b6603ea98de3)

Doing a quick search on google, we see "upx -d" is the command for unpacking an upx file.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/95191f8a-0733-423b-b556-22c29c83b7c9)

First, we need to download UPX from "https://github.com/upx/upx/releases/tag/v4.2.2". Follow the commands shown below. "chmod +x" is used to give upx permission to run.
![image](https://github.com/moromerx/Blue-Team/assets/162036545/e714a9fb-995d-46a4-b322-51f80c02d7ee)

Also, run "sudo mv upx /usr/bin/" so you can access "upx" from any directory (if an older version exists, remove it first). Alternatively, you can move the "upx" file to the directory with the "packed" file.

Now unpack the binary and save it with any name. I have saved it as a "file" here.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/6a409c78-d318-4c89-9c73-856aad0e963c)

Now use strings against the unpacked file. We will be able to see all the strings clearly as the file is no longer compressed.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/58932acf-8955-4ea3-85d2-7dddaeb4a623)

The flag can be seen among the strings.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/9132c181-4e70-41e7-a5ca-d0e3a1a83d27)

FLAG: HTB{unp4ck3d_th3_s3cr3t_0f_th3_p455w0rd}

Done! 🎉
