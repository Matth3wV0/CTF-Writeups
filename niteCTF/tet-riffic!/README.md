The challenge is provided with two files: tetris.pcapng and tetris.py.

![Screenshot 2024-12-17 144957](https://github.com/user-attachments/assets/1ce6df2c-21fd-4a25-96f8-829e0ae57bf4)

When running the tetris.py file, it becomes clear that it is a Tetris game, where the blocks are moved using keyboard keys such as A, D, S, W, etc.

```python
for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_w:
                game.rotate()
            if event.key == pygame.K_s:
                game.go_down()
            if event.key == pygame.K_a:
                game.go_side(-1)
            if event.key == pygame.K_d:
                game.go_side(1)
            if event.key == pygame.K_SPACE:
                game.go_space()
            if event.key == pygame.K_ESCAPE:
                game.__init__(810, 910)
```

![Screenshot 2024-12-17 145534](https://github.com/user-attachments/assets/09adc6b0-43da-45b9-a699-4b1f1f1da442)

After closely examining the tetris.py file, I noticed some unusual behavior. The blocks are not being generated randomly, as is typical for Tetris games. Instead, they are predetermined and appear in a fixed sequence.

```python
pre_blocks = [(0,1), (4,1), (0,2), (6,1), (7,2), (5,1), (3,2), (7,1), (7,1), (7,1), 
              (2,2), (7,2), (7,1), (7,1), (7,2), (7,1), (2,2), (7,2), (7,2), (7,1),
              (7,1), (7,2), (5,2), (5,1), (4,2), (7,2), (7,1), (7,2), (7,2), (7,2), 
              (7,1), (6,1), (5,2), (0,1), (2,1), (7,1), (7,2), (7,1), (7,1), (5,2),
              (7,1), (7,1), (7,1), (5,2), (7,1), (1,2), (4,2), (6,2), (3,1), (4,2),
              (0,1), (0,2), (0,2), (0,2), (6,1), (3,1), (7,1), (0,2), (0,1), (4,1),
              (0,1), (3,2), (5,2), (0,1), (7,2), (7,2), (7,1), (7,2), (7,2), (7,1),
              (5,2), (1,1), (7,1), (7,1), (7,2), (7,2), (7,1), (1,2), (7,1), (7,1),
              (7,2), (7,2), (3,2), (7,1), (0,1), (3,1), (3,2), (7,1), (7,1), (0,1),
              (5,1), (0,1), (4,2), (7,1), (4,2), (7,1), (7,2), (7,1), (2,2), (3,1),
              (3,1), (7,2), (6,1), (7,2), (7,2), (7,2), (7,2), (7,1), (7,1), (1,1),
              (3,1), (7,2), (7,2), (7,2), (7,2), (7,1), (7,1), (7,2), (7,2), (1,1),
              (4,2), (7,2), (4,2), (5,1), (5,2), (7,2), (7,2), (7,2), (7,2), (7,2),
              (5,1), (7,2), (5,1), (6,1), (6,1), (7,1), (7,1), (7,1), (7,1), (7,1),
              (6,1), (6,1), (0,1), (3,1), (6,2), (7,1), (7,1), (7,2), (7,2), (4,2),
              (2,1), (7,2), (7,2), (1,2), (7,1), (7,1), (5,1), (1,2), (7,1), (7,2),
              (4,2), (5,1), (3,1), (2,2), (7,2), (7,2), (7,1), (7,2), (7,1), (7,1),
              (7,2), (2,2), (7,1), (7,1), (1,2), (5,1), (7,1), (7,2), (7,2), (7,1), 
              (7,1), (7,1), (7,1), (7,1), (7,1), (7,2), (1,2), (7,1), (7,2), (7,2),
              (7,1), (7,1), (7,2), (4,2), (7,2), (7,2), (7,2), (4,1), (7,2), (4,2),
              (0,2), (3,1), (3,1), (6,1), (6,1), (4,1), (5,1), (7,2), (7,2), (7,2),
              (7,2), (7,2), (7,2), (7,1), (7,2), (7,2), (7,2), (6,1), (7,2), (7,2),
              (7,1), (7,1), (7,1), (0,1), (3,2), (0,2), (7,1), (7,1), (7,1), (7,2),
              (2,1), (2,2), (5,1), (7,1), (7,1), (7,1), (7,1), (7,1), (7,1), (7,1),
              (7,1), (7,2), (7,2), (7,2), (7,2), (7,2), (7,2), (7,2), (7,2), (5,1),
              (7,1), (6,1), (7,1), (7,2), (5,2), (7,1), (4,2), (4,1), (5,2), (7,2),
              (7,2), (7,2), (5,1), (5,1), (4,2), (7,1), (7,1), (7,1), (7,1), (7,2),
              (0,1), (3,2), (7,1), (7,1), (7,2), (7,1), (1,2), (0,1), (4,2), (7,1),
              (7,1), (5,2), (7,1), (7,2), (7,2), (7,1), (5,2), (7,2), (6,1), (7,2), 
              (7,1), (7,1), (7,1), (7,2), (7,2), (7,2), (5,2), (6,1), (4,2), (7,2), 
              (7,2), (7,2), (7,1), (0,1), (7,1), (7,2), (5,1), (4,2), (5,2), (4,2),
              (7,1), (5,2), (4,1), (4,1), (7,2), (7,2), (7,2), (2,2), (7,2), (4,2),
              (6,1), (6,1), (7,1), (7,1), (7,1), (7,1), (7,1), (7,2), (7,2), (1,1),
              (7,2), (7,1), (7,1), (6,2), (7,1), (5,2), (1,1), (7,2), (7,1), (5,2),
              (7,1), (0,1), (5,1), (7,2), (7,2), (7,2), (7,2), (7,2), (7,1), (0,1),
              (7,1), (5,1), (1,1), (7,1), (7,2), (7,2), (7,2), (4,2), (5,2), (7,1),
              (7,1), (5,1), (7,2), (7,2), (3,2), (7,1), (7,2)
              ]
```

```python
    def new_figure(self, counter):
        global b_counter
        b_counter = counter
        while b_counter < len(pre_blocks):
            a, b = pre_blocks[b_counter]
            b_counter += 1
            self.figure = Figure(a, b)
            return

        self.figure = Figure(0, 0)
```

Looking into the tetris.pcapng file, I observed that the URB requests captured data from the user’s keyboard. These keypresses correspond to the keys used in the Tetris game within the Python file.

![Screenshot 2024-12-17 145813](https://github.com/user-attachments/assets/f31f6ac4-debe-48d2-b5d7-d8bba3b91d07)
![Screenshot 2024-12-17 145731](https://github.com/user-attachments/assets/98e0dcc6-580f-4a6d-8f93-acf9bb4238a2)

From this, I deduced that the captured keystrokes might hold some valuable information. By attempting to reuse these captured keystrokes in the Python file, I might uncover something useful.

Thus, I decided to use tshark to extract usbhid.data from the pcapng file.

![Screenshot 2024-12-17 151536](https://github.com/user-attachments/assets/e582ad7d-00bb-46f6-9289-ac252774290d)

After this, I searched online and found a tool created by another user that could extract these captures and print them in plaintext for easy reading: [GitHub - Usb_Keyboard_Parser](https://github.com/5h4rrK/CTF-Usb_Keyboard_Parser/blob/main/Usb_Keyboard_Parser.py)

![Screenshot 2024-12-17 150838](https://github.com/user-attachments/assets/a70fcee8-4160-4b12-9c96-9b58740f49d7)

However, this code was not compatible with the specific challenge I was working on, so I made a few modifications to it.

```python
    for i in range(len(output)):
        buffer = str(output[i])[2:-1]
        # print(buffer[6:8])
        if (buffer)[:2] == "02" or (buffer)[:2] == "20":
            for j in range(1):
                count +=1 
                m ="0x" + buffer[6:8].upper()
                if m in usb_codes and m == "0x2A": message.pop(len(message)-1)
                elif m in usb_codes: message.append(usb_codes.get(m)[1])
                else: break
```

I successfully ran the updated script, and it output a series of keystrokes that appeared to be very useful for the task.

![Screenshot 2024-12-17 151655](https://github.com/user-attachments/assets/0d93de48-eeb2-4980-a496-f16e0721b902)

Next, I used these keystrokes in the tetris.py file to enable the game loop to run automatically.

```python
keystrokes = "WAAAAAAAAAAAAAAA AAAAAAAAAAAAWWW AAAAAAAAA AAAAAAAA AAAAAA AAAAAW AAAW AA A DD DA DDD DDDD DDD DDDD DDDDDDD DDDDD DDDDDDDD DDDDDDDDD DDDDDDDDDD DDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDDDDWW DDDDDDDDDDDDDDWWW DDDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDD DDDDDDDDDD DDDDDDDDDDD DDDDDDDD DDDDDDDDDDWW DDDDDDDDDDDDDW DDDDDD DDDDD DDDDD DDDDD DDDD DDDWWW DD D DA AWWW AAAA AAAW WWWAAAAA AAAAAAA AAAAAAAA AAAAAAAAAW AAAAAAAAAA AAAAAAAAAAA AAAAAAAAAAAAAAW AAAAAAAAAAAAAAA AAAAAAAAAAAAA AAAAAAAAAAAAAA AAAAAAAAAAAA AAAAAAAAAAAAAW AAAAAAAAAAAAAAAA WAAAAAAAAAAAAAAAA WAAAAAAAAAAAA AAAAAAAAAAWWW AAAAAAAAAAAAAAAA AAAAAAAAAAAAAAAA AAAAAAAAAAAAAAA AAAAAAAAAAAAAAA AAAAAAAAAAAAAAA AAAAAAAAAAAAA AAAAAAAAAAAA WWWAAAAAA WAAAAA WAAAA AA A AA A  D DD  D DD WWWDDDDD DDDDDDD WDDDDDDDDD WDDDDDDDDDDDDD WWWDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD WDDDD WDDDDDDD DDDDDDDD WWWDDDDDDDDD DDDDDDDDDD WDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDDD DDDDDDDDDDDDDDD WDDDDDDDDDDDDDDD WWDDDDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDDD  DDDDD DDDD DDD DD D AA WAAAAAAA AAAA AAAAA AAAAAA AAAAAAA AAAA  A AA A WW DDDDDDDDDDDDD WDDDDDDDDDDDD DDDDDDDDDD WDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDD DDDDDDDDD DDDDDDD DDDDD DDDDDD DDDDDD DDDDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDD DDDDDDDDD DDDDDDDDDD DD DD AAAAAAAAAAAAAA AAAAAAAAAAAA WAAAAAAAAA WWAAAAA AAAA AAAAAAA AAAAAAA AAAAAA AAAAAA WWWAAAAAAAAAA AAAAAAAAAA DDDDDDD DDDDD WDDD DDDD DDD WWW DD DDD DDDD WWD WDDDD WWDDDDDD WDDDDDDD DDDDD DDDDD DDDDDDDD DDDDDDDDD DDDDDDD DDDDDDDDD DDDDDDDD DDDDDDDDDD DDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDDD WDDDDDDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDDD DDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDD WDDDDDDDDDDD DDDDDDDD DDDDDDDDDD DDDDDDDDD DDDDDDDDDD DDDDDDDDDDD DDDDDDDDDDDD WWWDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDDDD DDDDDDDDDDDDDD WDDDDDDDDDDDDD DDDDDDDDDDD WDDDDDDD WDDDDDDDD WWDDDDDD DDDDD DDDDDDDDDDD DDDDDDDDDDDDD WWDDDDDDDD WWWDDDDDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDDD DDDDDDDDDD DDDDDDDD DDDDDDD DD DDD DDDD  D D D DD DDD DDDD WWWDD WDDDDDD DDDD DDDDD DDDDDD DDDDDDDD WDDDD WDDDDDD WWDDDDDDDDDD DDDDDDD DDDDDDDDD DDDDDDDDDDD DDDDDDDDDDDD DDDDDDDDDDDDD DDDDDDDDDDDDDD DDDDDDDDDDDDD DDDDDDDD DDDDDDDD DDDDDDDD DDDDD DDDDDDD DDDDDDDDDD DDDDDDDDDD DDDDDDDDDDD DDDDDDDDDDDD WWDDDDDD DDDDDDDDD DD D D WDDDDDDDDDDDDDDDD DDDDDDDDDDDDDDD WDDDDDDDDDDDDDD WWWDDDDDDDDD WWDDDDDDDDDDDD DDDDDDDDDD DDDDDDD DDDDD WWWDDDDDDDD WWDDDDDD WWDDDDDDDD DDDDDDDD DDDDDD DDDDDD DDDDDD DDDDDDD DDDD WDD DD DDD DDDDD D DD WDDDDDDDDDDDD WWWAAA A AAAA AAA AA AA A AAAAAA WAAAAA AAAAAAA A  A  AA AA AA AA WWA AA A D D D D DDDDDDDDDD DDD DDD WWWDD WDDD WWDD WWDDDDDD DDDDDDDD WDDDDDDDDD WDDDDDD WWDDD DDDDD DDDDD DDDDDDDDD DDDDDDD DDDDDDD WWWD DDDDD DDD  A AA AA AA A  WAAAA AAAA AAA AAAA AAAA AAAA WAAA WAAAA AAAA AAAA WWAAA A  AA A A A AAA AAAA AAAA WAAAA AA W D  D DD DD WWWAAAAA WWAA AAAA AAAA AA AAA A AAAAA AAAA AAAAA "  # Add your full keystroke string here
cursor = 0  # Cursor to track the current keystroke position

while not done:
    if game.figure is None:
        game.new_figure(b_counter) 
    counter += 1
    if counter > 100000:
        counter = 0

    if counter % (game.level // 2) == 0 or pressing_down:
        if game.state == "start":
            game.go_down()

    # Simulate automatic moves from keystroke string
    if cursor < len(keystrokes):
        current_key = keystrokes[cursor].lower()
        if current_key == 'w':
            game.rotate()
        elif current_key == 'a':
            game.go_side(-1)
        elif current_key == 'd':
            game.go_side(1)
        elif current_key == 's':
            game.go_down()
        elif current_key == ' ':
            game.go_space()
        cursor += 1  # Move to the next keystroke

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                game.__init__(810, 910)
```
At this point, I noticed the game generating a pattern resembling a QR code, indicating I was on the right track. However, there were still some issues to resolve, as the game’s height limit seemed to restrict the keystrokes.

![Screenshot 2024-12-17 152031](https://github.com/user-attachments/assets/9a847945-1ff4-41a6-b64a-2452950626b6)

After adjusting, I successfully obtained a partial QR code.

![Screenshot 2024-12-17 152259](https://github.com/user-attachments/assets/4974948d-5241-4058-a38a-1ed2992a4f18)

Then, when I tried to google to get some information, I found some useful information, realizing that a QR code doesn't need to be complete, but we can still read it if it has the essential key components are the three large squares (called the “finder patterns”) and the small square at the bottom right part of the QR is mainly used for alignment purposes of the QR code so that the scanner knows what is the orientation it is looking at. Same goes for the two lines (one horizontal and one vertical) called “timing patterns” indicated in yellow in my reconstructed QR code as below[^1].

![0_YNFHG-IMms4Bc1ve](https://github.com/user-attachments/assets/204939f0-2275-4a42-92b3-acadd5597157)

So I manually recreated this QR code in excel with pixel accuracy.

![Screenshot 2024-12-17 152334](https://github.com/user-attachments/assets/4aef63a3-3fe7-4253-820e-7c08d348fd1a)

Then use QRazyBox tool to read this incomplete QR code.

![Screenshot 2024-12-17 152352](https://github.com/user-attachments/assets/8063e5de-7d63-40cf-bb78-b32590936393)


And so I found the flag.

![Screenshot 2024-12-17 152414](https://github.com/user-attachments/assets/7dd77167-1750-4880-b65e-2f9317c0f62d)

> nite{mayb3_th3_r3al_tetr15_wa5_qrc0d3s_we_mad3_a10ng_th3_way}

[^1]: https://medium.com/@nteezy/how-to-decode-a-partially-visible-or-damaged-qr-code-a-ctf-writeup-for-stack-the-flags-2020-4ef0eb6a018f
