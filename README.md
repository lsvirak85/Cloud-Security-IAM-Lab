# Cloud-Security-IAM-Lab

# Lab: Creating Users with Least Privilege Access Keys

## Objectives

- Learn how to use the AWS Management Console
- Create IAM users with various privileges
- Learn how to manage access keys for those users

## Lab Setup

To perform this first lab all you need is an AWS account
## Lab - Step by Step Instructions

### 1: Log in to the AWS Management Console as your root user

1. Open the AWS Management Console:

https://console.aws.amazon.com/

2. Click "Sign in using root user email"

![Image](https://github.com/user-attachments/assets/c68ebdb0-d330-4c79-a040-ee0edbc48f71)

<p align="center"><i><b>AWS Management Console Login Page</b></i></p>

3. In the box titled "Root user email address" enter your AWS root email address, and then click Next.

4. Enter your password, and click "Sign in".

> **_NOTE:_** If you have already enabled MFA on your root account complete the MFA prompt to complete your sign-in. 

After you have successfully logged in you should now see the AWS Management Console home page. 

![Image](https://github.com/user-attachments/assets/ea78910f-4cb8-4d95-a9c7-c6a3efe23ee3)

<p align="center"><i><b>AWS Management Console</b></i></p>

### 2: Create a user with administrative permissions

Next, we will create a new IAM user with administrative permissions on your AWS account. 

1. In the search bar at the top of the AWS Management Console search for "IAM", and click the "IAM" service.

![Image](https://github.com/user-attachments/assets/37935270-31f3-4b75-b8a3-8db324bb06f0)

<p align="center"><i><b>Search for IAM</b></i></p>

2. On the IAM Dashboard page click on "Users" on the left.

![Image](https://github.com/user-attachments/assets/8211874a-d669-4ab8-9d85-7b9637f5140b)

<p align="center"><i><b>Click on Users</b></i></p>

3. Click "Create user"

4. Enter "AWSCloudAdmin" as the user name, and click Next.

![Image](https://github.com/user-attachments/assets/52fbccb2-912e-4d8b-a4d8-35b4e3c964cf)

<p align="center"><i><b>Create Your AWSCloudAdmin User</b></i></p>

> **_NOTE:_** The next page is where you can apply permissions to a user account during the IAM user creation process. AWS has a large number of default (managed) permission policies that can be utilized to provide permissions to an account. It is also possible to create custom policies as well. 

5. Click on "Attach policies directly", then click the "+" sign next to "AdministratorAccess"

![Image](https://github.com/user-attachments/assets/9cacb98a-89f1-421b-ae1f-cb0a62406f7a)

<p align="center"><i><b>Review the AdministratorAccess Policy</b></i></p>

6. Review the AdministratorAccess policy that is going to be applied to the AWSCloudAdmin user.

> **_Admin Policy_**
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

7. Click the checkbox next to the AdministratorAccess Policy, then click Next.

![Image](https://github.com/user-attachments/assets/31c34481-e0e9-474e-8cd5-d770cb9ed01a)

<p align="center"><i><b>Apply the AdministratorAccess Policy</b></i></p>

8. On the "Review and create" page review the details of the AWSCloudAdmin user and then click "Create user". 


### 3: Create access keys for the AWSCloudAdmin

So far, you have created a new IAM user with administrative permissions but this user does not have any credentials it can use to login yet. Throughout the labs in this workshop you will use this account to deploy resources. To do that you will need to add AWS access keys so that it can programmatically access your account. 

1. Click on the AWSCloudAdmin user. 

2. Click the "Security credentials" tab, then click "Create access key".

![Image](https://github.com/user-attachments/assets/ad62cbf6-5370-4f55-a431-939b7f35caa9)

<p align="center"><i><b>Create Access Keys</b></i></p>

3. Select "Command Line Interface (CLI)", click the check box to confirm, then click Next.

![Image](https://github.com/user-attachments/assets/de5a1fac-385e-411e-9c4d-de942ef4e438)

<p align="center"><i><b>Select CLI Access Keys</b></i></p>

4. Click "Create access key". 

5. On the next screen will be the newly created AWS access keys. You will need to securely store both the "Access key" and "Secret access key". Click the "Show" button under "Secret access key" to reveal it. You will be adding these to an AWS profile with the AWS command line tool. 

![Image](https://github.com/user-attachments/assets/3dc8f03b-8fcd-4921-8fba-8c885db88c6c)

<p align="center"><i><b>Successful Access Key Creation</b></i></p>

6. Click the "Terminal" icon on the left toolbar to open a new terminal window. If you already have a terminal window open, right-click the icon and select "New Window".

<div align="center">
  <img src="https://github.com/user-attachments/assets/8333e60d-4fb8-40ec-8583-c50ccecd045b">
</div>

<p align="center"><i><b>Open a Terminal</b></i></p>

7. Run the following command to create a new profile for the AWSCloudAdmin user. 

> **_Command_**
```bash
sudo aws configure --profile AWSCloudAdmin
```

> **_NOTE:_**
You will be prompted to enter your "fog" user's password which is also "fog".

8. When prompted for your Access Key ID, copy the "Access key" from the AWS Management Console and paste it in to the terminal. Hit enter, and then do the same thing for your "Secret access key". Leave "Default region name" and "Default output format" blank, hitting enter for both. 

![Image](https://github.com/user-attachments/assets/07d3a15a-0d1b-406f-9cba-02ba30abe5c4)

<p align="center"><i><b>Create AWSCloudAdmin Profile</b></i></p>

9. You can verify that you correctly added the credentials to a profile by running the following command. 

> **_Command_** 
```bash
sudo aws sts get-caller-identity --profile AWSCloudAdmin
```
![Image](https://github.com/user-attachments/assets/1e550d31-cf58-4f77-855c-d6b0e997596d)

<p align="center"><i><b>Check Access Keys</b></i></p>

> **_NOTE:_**
If you see any error messages such as "profile could not be found", or "access denied" messages the access keys were likely not copied into the profile correctly. Also, profiles are case-sensitive. Make sure you are using the correct capitalization for the AWSCloudAdmin profile. 


### 4: Create a lower privileged user with fewer permissions

In the previous steps you created a new user with administrative access keys from the AWS Management Console. Now you will leverage that account to create a new lower-privilege user. Instead of using the AWS Management Console you will use the AWS command line tool to do it.

1. In the same terminal you used previously run the following command to create a new IAM user called "s3user".

> **_Command_**
```bash
sudo aws iam create-user --user-name s3user --profile AWSCloudAdmin
```
![Image](https://github.com/user-attachments/assets/3e9dd094-89c8-4df9-9a2f-49e9d680e878)

<p align="center"><i><b>Create S3 User with CLI</b></i></p>

2. Attach the AmazonS3FullAccess policy to the new "s3user". This policy allows for full control over Amazon S3 buckets, including creating them and destroying them.

> **_Command_**
```bash
sudo aws iam attach-user-policy --user-name s3user --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --profile AWSCloudAdmin
```

Take a look at the policy we just applied below. Notice how the "Action" field is quite different than the "AdministrativeAcces" policy we applied previously. 

> **_S3 Full Access Policy_**
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*",
                "s3-object-lambda:*"
            ],
            "Resource": "*"
        }
    ]
}
```

> **_NOTE:_**
Note: If you want to see these policy documents for other policies you can run the following command and specify the name of the policy (AmazonS3FullAccess) or locate them in the Management Console.
`sudo aws iam get-policy-version --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --version-id v1 --profile AWSCloudAdmin`

3. Run the following command to create a new set of access keys for the "s3user".

> **_Command_** 
```bash
sudo aws iam create-access-key --user-name s3user --profile AWSCloudAdmin
```
![Image](https://github.com/user-attachments/assets/c5fe2f96-5104-4b85-9777-ba4d66102053)

<p align="center"><i><b>Create S3 User Access Keys</b></i></p>

4. Finally, set the new access keys to a new profile for the "s3user" with the AWS command line. Copy the AccessKeyId and SecretAccessKey from the previous command when prompted. Leave Default region name and output format blank.

> **_Command_** 
```bash
sudo aws configure --profile s3user
```
![Image](https://github.com/user-attachments/assets/fb5a9c5f-d2fd-4ebf-b21c-8a1257709b83)

<p align="center"><i><b>Add S3user Keys to Profile</b></i></p>

5. Remember that you can verify that you correctly added the credentials to a profile by running the following command. 

> **_Command_**
```bash
sudo aws sts get-caller-identity --profile s3user
```

## Conclusion

In this lab, you've learned how to create new IAM user accounts with both the AWS Management Console as well as the AWS command line. Additionally, you created a lower-privileged user account by applying a policy with only the minimum necessary permissions. When managing users always keep in mind the principle of least privilege to avoid over-privileged users. 
