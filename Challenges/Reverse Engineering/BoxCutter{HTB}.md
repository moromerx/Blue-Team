## ✂️ Box Cutter (Very Easy) - HTB

Follow along: [rev_boxcutter.zip](https://github.com/moromerx/Blue-Team/blob/main/Challenges/Reverse%20Engineering/Files/rev_boxcutter.zip)

Unzip the file

![image](https://github.com/moromerx/Blue-Team/assets/162036545/28cd8b9a-07ef-495d-a352-4334cdea8108)

We need to use 'strace' to solve this challenge.

'strace' is a tool used on Linux that helps you see what a program is doing in the background. Imagine you have a program that communicates with the computer to get things done. 
'strace' lets you listen in on this conversation. It tells you which requests (system calls) are being made by the program to the computer's operating system.
Read more about 'strace' here: [strace - Wikipedia](https://en.wikipedia.org/wiki/Strace#:~:text=strace%20is%20a%20diagnostic%2C%20debugging,and%20changes%20of%20process%20state.)

If you don't already have 'strace' installed, you can install it with "sudo apt install strace".

Now run "strace ./cutter" and you will see alot of output on the terminal. This is the "conversation" between the binary and the operating system's kernel. 

![image](https://github.com/moromerx/Blue-Team/assets/162036545/caa93340-6dbc-4deb-9de1-0a0cde5d3409)

We can see the flag in the output provided.

![image](https://github.com/moromerx/Blue-Team/assets/162036545/6b1e540c-d786-4b89-9bf4-46b08369db46)

FLAG: HTB{tr4c1ng_th3_c4ll5}

Done! 🎉
