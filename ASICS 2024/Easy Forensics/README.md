![Screenshot 2024-10-07 111733](https://github.com/user-attachments/assets/4ba819f4-8b1e-4b7d-9ebb-8b8150f22bd2)

The problem starts with the Memory.rar file, after extracting it we get 2 files Memory.raw and Gift.raw.

![Screenshot 2024-10-07 112002](https://github.com/user-attachments/assets/1170f784-f61f-4d0c-a5a0-159eb265e297)

Because this is a raw memory file => use volatility

![Screenshot 2024-10-07 111512](https://github.com/user-attachments/assets/38235985-43ab-44e6-a446-c58edc2b7528)

Find information, file profile

![Screenshot 2024-10-07 111417](https://github.com/user-attachments/assets/8ec04db8-1e5a-4ab0-88e1-d5b94b3d7d18)
![Screenshot 2024-10-07 111432](https://github.com/user-attachments/assets/8f304d57-97f0-4bfe-906d-9ca000746aeb)

I used scan, list to find if there are any unusual files. But the result was I found nothing.

![Screenshot 2024-10-07 112200](https://github.com/user-attachments/assets/1fbad804-68c9-4111-9357-16934c0f842c)

Based on the given data, I researched that persistence technique will use some ways for malware to turn on and start automatically when the system restarts => maybe the malware has modified Windows Registry or Startup folder,...

![Screenshot 2024-10-07 111141](https://github.com/user-attachments/assets/d6530efb-e40e-42e3-9828-e79255d95355)

Malware often adds to the "Run" folder of the Windows Registry to automatically launch when the system restarts. So I used Volatility to check the Windows Registry for any suspicious folders

And luckily I found a base64 encrypt code:
>UGFzc3dvcmQgaXMge0ZpbGVsZXNzLU1hbHdhcmUtUGVyc2lzdGVuY2V9

![Screenshot 2024-10-07 112956](https://github.com/user-attachments/assets/00799e47-5d28-4cac-b45d-8a208a9a45f8)

Decrypted it and I got the password of the Gift.rar file.

![Screenshot 2024-10-07 113306](https://github.com/user-attachments/assets/11c658df-eeab-4faa-ac65-1b82a365de9d)

Extract Gift.rar file, I got flag in Gift.txt file:
>ASCIS{Gh4st1n_Th3_R2M}
