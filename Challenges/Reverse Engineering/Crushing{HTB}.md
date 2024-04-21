## 🔨 Crushing (Easy) - HTB

Unzip the file.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/716bfb50-d34f-4fe3-a6fe-3d5412d28876)

The folder contains two files, a binary and a text file which is possibly compressed or encoded considering ".cz".

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/36eff18e-6b63-492e-8749-2f2473c97c0c)

I tried running the 'crush' file (using ./crush) but nothing seems to be displayed.

Let's decompile the binary using Ghidra.

Find the main function.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/6afc6cb3-964e-4d7f-b1bb-016d2e301d14)

After understanding the function a bit, I have renamed the variables:

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/fda2d58d-0ac8-434b-b00c-b777b00e4e14)

The main function:

1. It declares a set of variables, including a long integer i, a pointer puVar1 to an undefined8 type (8 bytes in length), an array of undefined8 type with 256 elements, an unsigned integer input, and a long integer count.

2. It sets the puVar1 pointer to the start of the array.

3. It enters a loop to initialize the array. This loop runs backwards from the last element (0xff, which is 255 in decimal) to the first element (0), setting each element of the array to 0.

4. It sets the count variable to 0. Then, it enters a while loop where it:
- Reads a character from the standard input (user's input) using the getchar() function and stores it in the input variable.
- If the input is equal to 0xffffffff (or '-1', which indicates EOF or an error), it breaks out of the loop.
- Calls the add_char_to_map function, passing the array, input & 0xff, and count as arguments.  In C, a char typically represents a single byte, and using bitwise 'AND' with 0xff ensures that if input is of a larger type, only the least significant byte is considered.
- Increments the count.

5. After the loop ends (after the EOF input), it calls the serialize_and_output function, passing the array.

6. Finally, the function returns 0 and the program ends.

We still don't really get what the program is doing; let's check the 'add_char_to_map' function (double-click on it).

`Note: I changed the name of the first two variables to array and input.`

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/ecc3e192-85ea-4550-a1f2-3fbd083eeac3)

The add_char_to_map function:

`Note: The concept of linked list is used here.`

1.  It defines a pointer puVar1 and a long integer local_10.

2.  local_10 = *(long *)(array + (ulong)input * 8); - This line of code first takes the character input (converts it to an unsigned long int) and multiplies it by 8 to find the correct byte offset in the array. This is because each element in the array is 8 bytes long. By multiplying the character code by 8, we are calculating the starting address of the slot in the array that is reserved for this particular character. Then, the code dereferences the calculated address to retrieve the value stored at that location in the array, and assigns this value to the variable local_10.

3.  puVar1 = (undefined8 *)malloc(0x10); - It then allocates 16 bytes (0x10 in hexadecimal) of memory and assigns the address of this memory to puVar1. This is because the first 8 bytes will be used to store information and the next 8 bytes will store the address of the next node. (Linked list Concept)

4.  *puVar1 = param_3; - It sets the first undefined8 unit of this allocated memory to the value of param_3, which is passed in from the main function as count. This is basically the information part of the node.

5.  puVar1[1] = 0; - This sets the second element, which will be the address of the next node, to zero.

6.  if (local_10 == 0) { *(undefined8 **)((ulong)input * 8 + array) = puVar1 } - This checks if the value at local_10 is 0 or not. If it is, it means this character has not been entered before, and it creates a node for it. It stores the pointer puVar1 in the array, at the index for the character (where the character is supposed to be stored in the array).

7.    else { for ( ; *(long *)(local_10 + 8) ... *(undefined8 **)(local_10 + 8) = puVar1; } - If the local_10 variable is not equal to zero, this means this character has been entered before, and so it goes through the linked list till it reaches the last node and adds the character there (again, the linked list concept).

So the 'add_char_to_map' function uses a linked list to track occurrences of characters in a dataset. It calculates the position for a character in an array, checks if a linked list already exists there, and either creates a new list or appends to the existing one. 

The final function we need to understand is the searilize_and_output function:

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/adcc507c-763d-423a-9836-986f4cadb113)

1.  It starts by declaring some variables that will be used within the function.

2.  for (local_c = 0; local_c < 0xff; local_c = local_c + 1) - This for-loop will go through all the characters from 0-255 as the array (we passed it as an argument) has 256 elements and it loops till 255 (0xff). The numbers 0-255 can be mapped to characters on the ASCII table.

3.  local_20 = (void **)(param_1 + (long)local_c * 8); - Inside the loop, it calculates the address to access the linked list corresponding to the current character code: local_20 = (void **)(param_1 + (long)local_c * 8);. This is similar to what we saw in the add_char_to_map function, where it multiplies the character code by 8 to get the correct offset, then adds this to the base address (param_1).

4.  list_len(local_20); - This returns the length of the list.

5.  fwrite(&local_28,8,1,stdout); - The function writes the length of the list to the standard output (the console).

6.  for (local_18 = *local_20; local_18 != (void *)0x0; local_18 = *(void **)((long)local_18 + 8)) { fwrite(local_18,8,1,stdout); } - The inner for-loop iterates through each node in the linked list. It stops when it encounters a pointer that is NULL (0x0), which signifies the end of the list. It writes the data pointed to by local_18 to the standard output.

Now we have a good understanding of what the program does. It reads input character-by-character until EOF and uses a function add_char_to_map to build a data structure that maps each byte to a linked list of positions where that byte appears in the input. It tracks the frequency and position of each byte encountered. Finally, the program serializes this mapping into a structured format using serialize_and_output, where it outputs the length of each list followed by the position data for each byte, and writes this serialized data to a file.

Looking at the text file provided to us, it's likely the output for this program. Use "hexdump message.txt.cz | less" to view the content of the file in hexadecimal format.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/812de3d6-dfa7-43e2-9e11-7fc745d7cd68)

The "0000050" is the address. "000c 0000 0000 0000" are the first 8 bytes (00 is one byte). "000c" is 12, which means the first character has 12 occurrences. "0049" is 73, so the first position was 73, and so on.

We can decode this data by writing a python script:

from struct import unpack

content = bytearray(1024)

`Source: [HTB: solve.py](https://github.com/hackthebox/cyber-apocalypse-2024/blob/main/reversing/%5BEasy%5D%20Crushing/htb/solve.py)
```
fp = open("message.txt.cz", "rb")
highest = 0
for current in range(256):
    length_bytes = fp.read(8)
    if len(length_bytes) != 8: break
    length = unpack("Q", length_bytes)[0]
    for i in range(length):
        pos = unpack("Q", fp.read(8))[0]
        content[pos] = current
        highest = max(highest, pos)
print(content[:highest].decode())
```

1. from struct import unpack - This imports the unpack function from the struct module, which is used to interpret strings as packed binary data.

2. content = bytearray(1024) - This line initializes a bytearray of size 1024 bytes.

3. fp = open("message.txt.cz", "rb") - Opens the file in binary read mode.

4. highest = 0 - Initialized to 0 and is used later to track the highest index in the bytearray that has been modified.

5. for current in range(256): ... length = unpack("Q", length_bytes)[0] - Enters a loop that runs 256 times. It reads 8 bytes from the file. These 8 bytes represent a length, packed as a 64-bit unsigned integer (Q format). If fewer than 8 bytes are read (indicating the end of the file or an unexpected format), the loop breaks. The unpack function is used to convert these 8 bytes into an integer (length), which specifies how many positions will be set to the value of current in the bytearray.

6. for i in range(length): ... highest = max(highest, pos) - For each value from 0 to length-1, another 8 bytes are read and unpacked to get pos, the position in the bytearray that should be set to current. The current value (ranging from 0 to 255) is assigned to content[pos]. 'highest' is updated to track the highest position modified in the bytearray.

7. print(content[:highest].decode()) - After the loop completes, the code prints out the contents of the bytearray up to the highest index that was modified, decoding it from bytes to a string.

This will return a conversation between two organizers that includes the flag.

![image](https://github.com/moromerx/CTF-Challenges/assets/162036545/de1b17ca-7d22-4740-807b-0b16ce1242dd)

Done!🎉
