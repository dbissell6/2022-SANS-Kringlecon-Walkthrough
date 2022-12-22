Tolkein Ring

Chest under table in room
Chest outside tolkien 2 steps up ladder and left


# Windows Event Logs 
Windows Event Logs was a challenge where we are given powershell logs and asked 10 questions that we can find the answer from the logs. 
![[Pasted image 20221207150521.png]]

Question 1 

first answer is 12/24/2022

found by grepping for the commands a hacker would use when first getting on a system
```
ipconfig
whoami
pwd
```
![[Pasted image 20221207155022.png]]

Question 2 a file was saved to a variable, what was the full line that did that
```
$foo = Get-Content .\Recipe| % {$_ -replace 'honey', 'fish oil'} $foo | Add-Content -Path 'recipe_updated.txt
```
3) The attacker created a new file after modifyiing the original, what was the line that was used to create a new file?
![[Pasted image 20221207155742.png]]
```
$foo | Add-Content -Path 'Recipe.txt'
```
![[Pasted image 20221207161945.png]]

```
recipe_updated.txt
```

![[Pasted image 20221207162121.png]]
Recipe.txt

![[Pasted image 20221207162303.png]]

7 was the og file deleted? Yes

![[Pasted image 20221207162344.png]]
4104

![[Pasted image 20221207162855.png]]

![[Pasted image 20221207162913.png]]
![[Pasted image 20221207163112.png]]

``` 
echo + mydiary.txt reveals some story
```


# Suspicious pcap

5172 total packets


Question #1 there are objects that can be exported, what are they? HTTP
![[Pasted image 20221207223714.png]]
![[Pasted image 20221207223807.png]]
![[Pasted image 20221207223821.png]]
687
![[Pasted image 20221207223903.png]]
A: 192.185.57.242

5. What file is saved to the infected host?
A = Ref_Sept24-2020.zip
To get this, export the app.php file and run strings on it
![[Pasted image 20221208073652.png]]
![[Pasted image 20221208073716.png]]
Isreal + South sudan 
![[Pasted image 20221208124356.png]]

7) Is the host infected? Yes
![[Pasted image 20221208124446.png]]  






# Suricata Inncantations
hint - offical source
https://suricata.readthedocs.io/en/suricata-6.0.0/rules/intro.html
![[Pasted image 20221208154129.png]]
```
alert dns any any -> any any (msg:"Known bad DNS lookup, possible Dridex infection"; dns_query; content:"adv.epostoday.uk"; nocase;)
```


![[Pasted image 20221208154306.png]]
Question 2: Develop a Suricata rule that alerts whenever the infected IP address 192.185.57.242 communicates with internal systems over HTTP.

for this one i kept getting a same signature error, so i changed a previous rule, 3rd one down I think the signature error is i needed to have unique sid:xxxxx
```
alert http any any -> any any (msg:"Investigate suspicious connections, possible Dridex infection"; sid:2200073; rev:2;)
```

![[Pasted image 20221211162818.png]]

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
