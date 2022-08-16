# Rev-Bin!
### by Chetanya Bhan

## Problem statement
A password is hidden in some form in this binary file. Your task is to reverse engineer the binary to get the password. 
Write a detailed writeup (containing the password) as a markdown file, push any associated code into a private github repository, make it public ONLY after the deadline!

You may use any programming language, binary exploitation tool to solve this task. 


Firstly i analysed the file type using the file command.
<br>
<img width="482" alt="Screenshot 2022-08-16 at 11 34 04 PM" src="https://user-images.githubusercontent.com/105980346/184948327-9fca5bc9-1e1f-4b15-a976-b76df9c7a3d4.png">
<br>
Now i'll try running the file.
<br>
<img width="482" alt="Screenshot 2022-08-16 at 11 38 03 PM" src="https://user-images.githubusercontent.com/105980346/184948988-bece4571-0659-4265-b128-40d2e7891871.png">
<br>
Using the string function reveals to us:
<br>
https://user-images.githubusercontent.com/105980346/184949348-4c519b4a-3308-43a1-b423-a5e0bc21dca4.mov
<br>
which is not really useful.

So Ill open the file in Ghidra and try analysing the binary there.

## Ghidra
Going thru the main function reveals some information to us. After making some changes in the decompiled code heres what we end up with.
### main
<br>
<img width="589" alt="Screenshot 2022-08-16 at 11 43 39 PM" src="https://user-images.githubusercontent.com/105980346/184949873-10c468e0-0659-45aa-9833-7d95dcb3c4d1.png">
<br>
Our next stop has to be the checkPassword function.
### checkPassword function
It reveals that a stack of 31 elements is being declared which might just be the length of our password.
<br>
<img width="823" alt="Screenshot 2022-08-16 at 11 46 30 PM" src="https://user-images.githubusercontent.com/105980346/184950331-ca3f0bb1-ce44-49e4-b0d4-8047b14df33b.png">
<br>
These elements are assigned values later on which seemed very random at first.
<br>
<img width="562" alt="Screenshot 2022-08-16 at 11 47 58 PM" src="https://user-images.githubusercontent.com/105980346/184950611-fc0cbb07-25e1-4c2c-9af1-4a43513ae458.png">
<br>
But these stack is being manipulated as well as being compared to our input password as we can see in later functions.
<br>
### Process function
<br>
<img width="562" alt="Screenshot 2022-08-16 at 11 50 04 PM" src="https://user-images.githubusercontent.com/105980346/184950949-fdaf2e37-2414-451b-bcbb-c36ae5784422.png">
<br>
### Prepare function
This reveals more interesting details. It seems like some global variable has more info about the password but unfortunately it contains nothing but garbage seeming values
<br>
<img width="562" alt="Screenshot 2022-08-16 at 11 52 17 PM" src="https://user-images.githubusercontent.com/105980346/184951305-187404bd-8870-4792-861f-f9b56246076b.png">
<br>
### Verify function
This function again hints at the password being somewhere outside the scope of this function or at least being compared to it.
<br>
<img width="562" alt="Screenshot 2022-08-16 at 11 55 30 PM" src="https://user-images.githubusercontent.com/105980346/184951825-8f2bda42-863e-4b45-9120-0afb864cf114.png">
<br>
### format function
This was by far the most interesting function as it made conclude that i need to reconstruct the functions back to find the key as the password is going thru a binary shift and being assigned to the weird stack we saw earlier.
<br>
<img width="562" alt="Screenshot 2022-08-16 at 11 59 11 PM" src="https://user-images.githubusercontent.com/105980346/184952473-c35b400d-9386-43c0-b90f-7bfa07be9fdb.png">
<br>
### checkRes function
When the stack is again being compared to an out of scope stack/variable. My suspections are more or less confirmed about the working of the code
<br>
<img width="562" alt="Screenshot 2022-08-16 at 11 57 39 PM" src="https://user-images.githubusercontent.com/105980346/184952195-79ec19e5-c566-4bbe-ae96-c7601a9bb4ca.png">
<br>
## Conlusion
I think i came pretty close to solving this task...
**HOWEVER** due to the issues caused by the binary to even open it, I was left with a lack of time to solve this task. Implementing rev engineered functions wouldve taken too much of time. But I believe i learnt a lot from this and would defo take part in ctf competitions like this.
