![Screenshot 2024-10-11 160525](https://github.com/user-attachments/assets/aa504d9c-cf18-4a52-9369-a9148f484400)
We are given a series of files in a directory and provided with a SHA-256 hash value. The task is to calculate the checksums of the files in that directory and compare them with the given hash value.

![Screenshot 2024-10-03 103823](https://github.com/user-attachments/assets/9317ada8-38a1-4755-85bd-05258cd8c881)

From the above deduction, I will write a piece of code to iterate through all the files in the files directory, using checksums to compare them with the provided hash value

![Screenshot 2024-10-03 103624](https://github.com/user-attachments/assets/42ab549a-cf90-4552-9912-44f34f1541dc)

Here, I found the file 8eee7195 whose hash matches the hash provided in the challenge.

![Screenshot 2024-10-03 103641](https://github.com/user-attachments/assets/2a1b59f6-6999-4d3d-abb7-9ebcf2f86ca7)

And using the provided decrypt.sh file, I was able to find the flag located in the 8eee7195 file.

> picoCTF{trust_but_verify_8eee7195}
