---
layout: post
title: Trollcat CTF Feb 2021
tags: [CTF, 2021]
comments: true
---

Over the weekends, I participated in TrollCat CTF, a CTF organized by CSCodersHub in India and [placed 23rd](https://ctftime.org/event/1257). Here are the write-ups for the challenges, including some challenges that I did not manage to solve during the CTF itself. Enjoy! 

## Stegnography

### Change My Mind

{: .box-note}
Change my mind. [Download File](https://drive.google.com/file/d/1KNqvQgkAwASizOwXiTkan4KXmoRIEj7W/view?usp=sharing).

![Alien Message Waveform](../assets/img/2021-02-08-Trollcat-CTF/ChangeMyMind.png){: .mx-auto.d-block :}

This is the image we have been provided with. Running the image with zsteg, we can see the flag.

![Alien Message Waveform](../assets/img/2021-02-08-Trollcat-CTF/ChangeMyMind_Flag.png){: .mx-auto.d-block :}

### Alien Message

{: .box-note}
A Space Agency has got an unknown audio signal they captured it in file. Help them to decode the message. [Download File](https://drive.google.com/file/d/1AQEw7sP4e8WdRnLxMhjIr4Nzeh5OKgJT/view?usp=sharing).

Opening the mp3 file in Audacity and playing the song, we observe that there are beeping sounds in the middle of the song as shown in the following waveform.

![Alien Message Waveform](../assets/img/2021-02-08-Trollcat-CTF/AlienMessage.png){: .mx-auto.d-block :}

The beeping sounds resemble morse code. Converting the waveform to morse code by representing the wider bars as "-" and thinner bars as ".", we get the following morse code: "- .-. --- .-.. .-.. -.-. .- - -.-. - ..-. -... .-. --- ..- --. .... - - --- -.-- --- ..- -... -.-- -.-. ... -.-. --- -.. . .-. ... .... ..- -...". When decoded, it gives us the flag "TROLLCATCTFBROUGHTTOYOUBYCSCODERSHUB".

### Trolling Cat

{: .box-note}
Don't mess with my cat. [Download File](https://drive.google.com/file/d/1OlHig8YlpeZ2KlbsYSgAP0jrjSz8JrFt/view?usp=sharing).
You Might Find Password on my social media if you need it ;)

Opening the .rar file, we see a .png image inside. 

![Rar file](../assets/img/2021-02-08-Trollcat-CTF/TrollingCat_Rar.png){: .mx-auto.d-block :}

We extract the image and run binwalk. We see that there is a zip file inside which seems suspicious.

![Binwalk](../assets/img/2021-02-08-Trollcat-CTF/TrollingCat_Binwalk.png){: .mx-auto.d-block :}

We then extract the zip file using binwalk.

![Binwalk extract](../assets/img/2021-02-08-Trollcat-CTF/TrollingCat_Binwalkextract.png){: .mx-auto.d-block :}

Opening the zip file, we see that there is a flag.txt inside but it is password protected.

![Zip](../assets/img/2021-02-08-Trollcat-CTF/TrollingCat_Zip.png){: .mx-auto.d-block :}

Since the challenge description has mentioned that the password can be found on the author's social media account, we searched his instagram account and found a string that looks like the password on his bio.

![Password](../assets/img/2021-02-08-Trollcat-CTF/TrollingCat_Zippassword.png){: .mx-auto.d-block :}

Unlocking the flag.txt with the password, we can see the flag!

![Flag](../assets/img/2021-02-08-Trollcat-CTF/TrollingCat_Flag.png){: .mx-auto.d-block :}

## Forensics

### Forbidden

{: .box-note}
Agent Troll recieved some file but not able to read the data can you help us? [Download File](https://ctf.cscodershub.tech/files/c46ffc7ac0a5a27387b4a35f04671302/trollcats.car?token=eyJ1c2VyX2lkIjo4NDUsInRlYW1faWQiOjIzMywiZmlsZV9pZCI6Mzh9.YCOYKA.eKvCW18yYhXcO23zhasLXa3MU94).

Running binwalk on the .car file, we can see that it contains a bzip2 compressed file.

![Binwalk](../assets/img/2021-02-08-Trollcat-CTF/Forbidden_Binwalk.png){: .mx-auto.d-block :}

We use binwalk to extract the bzip2 file using the `--d=".*"` flag. Using bzip2 to unzip the bzip2 file, we `cat` the data in the file and obtained the flag!

![Flag](../assets/img/2021-02-08-Trollcat-CTF/Forbidden_Flag.png){: .mx-auto.d-block :}

## the_sus_agent

{: .box-note}
One of our agent is doing something suspicious on the network can you find out? [Download File](https://mega.nz/file/DxUmUToR#ckGf6JffCW2M7TixQzcfQNx9Ki-66gXyNSA4lUX5Ooc).

Opening the .pcapng capture, we can see that there are a lot of TCP packets with some HTTP communication. I exported the HTTP objects and found 2 files which seemed interesting, 2 .jpg images secret.jpg and welcome.jpg.

We were unable to open secret.jpg by double-clicking the image, suggesting that the file extension is incorrect. Running file on secret.jpg, we can see that it contains ASCII text which is the string "aWhvcGV5b3VkaWRub3R0cmllZHRvYnJ1dGVmb3JjZWl0". Decoding the string using CyberChef by base64, we get the string "ihopeyoudidnottriedtobruteforceit" which looks like a password of some sort.

![Password](../assets/img/2021-02-08-Trollcat-CTF/the_sus_agent_Password.png){: .mx-auto.d-block :}

Suspecting that the flag is hidden in welcome.jpg which requires a password to extract it, I ran steghide on welcome.jpg and passed "ihopeyoudidnottriedtobruteforceit" as the passphrase. 

![Steghide](../assets/img/2021-02-08-Trollcat-CTF/the_sus_agent_Steghide.png){: .mx-auto.d-block :}

Opening the file "foryou", we can see the flag!

![Steghide](../assets/img/2021-02-08-Trollcat-CTF/the_sus_agent_Flag.png){: .mx-auto.d-block :}

## s3cr3t

{: .box-note}
After getting trolled alot by Mr.Troll we finally got some files and now he's hiding some secret with him your mission is to find that secret. [Challenge file link.](https://mega.nz/file/PtsFHYzY#tKDykxlC1Uj5FniYU947AMRFJubc8OL11l0jmMbxmbA)

The file is a .tar.gz compressed file which contains an .E01 image file. Extracting the file and running it on Autopsy, we see that there is a deleted file topsecret.vhdx. 

![Bitlocker](../assets/img/2021-02-08-Trollcat-CTF/s3cr3t_Autopsy.png){: .mx-auto.d-block :}

We extract topsecret.vhdx and attempt to mount it by double clicking on it. However, we got a pop-up asking for the BitLocker password.

![Bitlocker](../assets/img/2021-02-08-Trollcat-CTF/s3cr3t_Bitlocker.png){: .mx-auto.d-block :}

While searching online on how to crack the BitLocker password, I came across a tool [bitcracker](https://github.com/e-ago/bitcracker) which can obtain the password hash from the BitLocker encrypted drive. The password can then be recovered by cracking the hash using John the Ripper.

Running bitcracker on topsecret.vhdx, we obtain the hashes which is stored in hash_user_pass.txt.

![Hash](../assets/img/2021-02-08-Trollcat-CTF/s3cr3t_Hash.png){: .mx-auto.d-block :}

We then crack the hash using John the Ripper and got the password "johncena".

![Drive](../assets/img/2021-02-08-Trollcat-CTF/s3cr3t_Password.png){: .mx-auto.d-block :}

Using the password to mount the tool, we opened the drive only to find a file named "please dont open it.txt" which contains "Try Harder !!! :{".

![Drive](../assets/img/2021-02-08-Trollcat-CTF/s3cr3t_Vhdx.png){: .mx-auto.d-block :}

I then opened the drive in Autopsy and found a deleted file dont_open_it.txt which contains the flag!

![Flag](../assets/img/2021-02-08-Trollcat-CTF/the_sus_agent_Flag.png){: .mx-auto.d-block :}






















