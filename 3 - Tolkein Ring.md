Tolkein Ring

Chest under table in room
Chest outside tolkien 2 steps up ladder and left


# Windows Event Logs 
Windows Event Logs was a challenge where we are given powershell logs and asked 10 questions we can answer by scouring the logs. 




![Pasted image 20221207150521](https://user-images.githubusercontent.com/50979196/209185371-72bedd76-f9de-4548-b89a-7f5d39160bcf.png)

![Pasted image 20221207133659](https://user-images.githubusercontent.com/50979196/209188340-1f74fd76-8943-4dc7-8199-c1157f81cbb1.png)

first answer is 12/24/2022

found by grepping for the commands a hacker would use when first getting on a system
```
ipconfig
whoami
pwd
```
![Pasted image 20221207155022](https://user-images.githubusercontent.com/50979196/209185487-b0f82a6a-24dd-41b1-9af8-90e63d658e81.png)

Question 2 a file was saved to a variable, what was the full line that did that
```
$foo = Get-Content .\Recipe| % {$_ -replace 'honey', 'fish oil'} $foo | Add-Content -Path 'recipe_updated.txt
```
3) The attacker created a new file after modifyiing the original, what was the line that was used to create a new file?
![Pasted image 20221207155742](https://user-images.githubusercontent.com/50979196/209185546-87c8f0ab-0b21-4100-a2d0-8b4ab4838167.png)

```
$foo | Add-Content -Path 'Recipe.txt'
```
![Pasted image 20221207161945](https://user-images.githubusercontent.com/50979196/209179581-e7f2ab4c-602b-4c0e-9b56-73a25bc701c2.png)

```
recipe_updated.txt
```
![Pasted image 20221207162121](https://user-images.githubusercontent.com/50979196/209185644-33b9b620-9078-434a-aa15-e34637b94745.png)

Recipe.txt

![Pasted image 20221207162303](https://user-images.githubusercontent.com/50979196/209185695-46ff6f46-9021-4828-bed1-fe2bd222393e.png)

Question 7 was the og file deleted? Yes

![Pasted image 20221207162344](https://user-images.githubusercontent.com/50979196/209186295-fdf9f584-4b5f-4880-bb4c-d6656df56f66.png)

4104

![Pasted image 20221207162855](https://user-images.githubusercontent.com/50979196/209186614-2a1c3462-dbb3-49f3-8696-9d2a7d3604ed.png)
![Pasted image 20221207162913](https://user-images.githubusercontent.com/50979196/209186683-e877317a-5090-4d2d-827d-9eecab3b2a23.png)
![Pasted image 20221207163112](https://user-images.githubusercontent.com/50979196/209186726-aa83a311-8317-49dd-9753-79ff0bc2a3df.png)

``` 
echo + mydiary.txt reveals some story
```


# Suspicious pcap

5172 total packets


Question #1 there are objects that can be exported, what are they? HTTP
![Pasted image 20221207223714](https://user-images.githubusercontent.com/50979196/209190588-acf2562b-db5e-4c30-a887-d369850f3862.png)

![Pasted image 20221207223807](https://user-images.githubusercontent.com/50979196/209190651-4b08de98-5af9-4099-bc10-7a9b76c3aa1f.png)

![Pasted image 20221207223821](https://user-images.githubusercontent.com/50979196/209190684-af6887a0-da76-4b4e-a90b-2a6e7e157603.png)

687

![Pasted image 20221207223903](https://user-images.githubusercontent.com/50979196/209190782-77eef543-76e7-441c-a3e3-1fc92e838a46.png)

A: 192.185.57.242


5. What file is saved to the infected host?
A = Ref_Sept24-2020.zip

To get this, export the app.php file and run strings on it

![Pasted image 20221208073652](https://user-images.githubusercontent.com/50979196/209190863-d6b78242-520f-4189-832e-88c18d56fd01.png)
![Pasted image 20221208073716](https://user-images.githubusercontent.com/50979196/209190933-ce149d1e-f217-43c7-b7c5-5810bcf23577.png)

Isreal + South sudan 

![Pasted image 20221208124356](https://user-images.githubusercontent.com/50979196/209190994-ee39a1fb-fc94-45fc-9b51-8604a72046e9.png)

7) Is the host infected? Yes

![Pasted image 20221208124446](https://user-images.githubusercontent.com/50979196/209191030-c9d4cbfe-949a-4e7e-a138-da84c98edf77.png)



# Suricata Inncantations
hint - offical source
https://suricata.readthedocs.io/en/suricata-6.0.0/rules/intro.html

![Pasted image 20221208154129](https://user-images.githubusercontent.com/50979196/209191814-b458f85a-bdc4-4cc7-8b51-37b20bc8a5f2.png)
```

alert dns any any -> any any (msg:"Known bad DNS lookup, possible Dridex infection"; dns_query; content:"adv.epostoday.uk"; nocase;)
```


![Pasted image 20221208154306](https://user-images.githubusercontent.com/50979196/209191860-f099307e-ae56-4b6f-bb08-9c0c4bd75e5b.png)

Question 2: Develop a Suricata rule that alerts whenever the infected IP address 192.185.57.242 communicates with internal systems over HTTP.

for this one i kept getting a same signature error, so i changed a previous rule, 3rd one down I think the signature error is i needed to have unique sid:xxxxx
```
alert http any any -> any any (msg:"Investigate suspicious connections, possible Dridex infection"; sid:2200073; rev:2;)
```

![Pasted image 20221211162818](https://user-images.githubusercontent.com/50979196/209191935-8244a305-73aa-43fa-a5e1-2b59d1bd9373.png)


```
We heard that some naughty actors are using TLS certificates with a specific CN.
Develop a Suricata rule to match and alert on an SSL certificate for heardbellith.Icanwepeh.nagoya.
When your rule matches, the message (msg) should read Investigate bad certificates, possible Dridex infection

```

Changed 6th line to this
```
alert tcp any any -> any any (msg:"Investigate bad certificates, possible Dridex infection"; flow:established; tls.cert_subject; content:"heardbellith.Icanwepeh.nagoya"; nocase; pcre:"/heardbellith.Icanwepeh.nagoya.$/"; sid:2260004; rev:2;)
```

Final answer

```
alert http any any -> any any (msg:"Suspicious JavaScript function, possible Dridex infection";  content:""; file_data;  sid:10000005;)
```

