## Crushing (Easy) - HTB

Unzip the file.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/716bfb50-d34f-4fe3-a6fe-3d5412d28876)

It looks like the folder contains these two files. A binary and a large text file.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/36eff18e-6b63-492e-8749-2f2473c97c0c)

I tried running the 'crush' file (using ./crush) but nothing seems to be displayed.

Let's decompile the binary using Ghidra.

Find the main function.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/6afc6cb3-964e-4d7f-b1bb-016d2e301d14)

