# devops-jenkins-eks-node

Deploy NodeApp to AWS EKS using Jenkins Pipeline.

## Install Jenkins

To install jenkins you can use the followin commandss:
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

Ps: If youmpan to use it as container or insttal in a different system, check https://www.jenkins.io/doc/book/installing.

## Install eksctl, kubectl and others

Install eksctl:
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

Move eksctl setup to /usr/local/bin
```
sudo mv /tmp/eksctl /usr/local/bin 
```

Check version:
```
eksctl version
```

Install kubectl:
```
sudo touch /etc/apt/sources.list.d/kubernetes.list 
```

Add K8S official repo:
```
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
```

Update packages:
```
sudo apt-get update
```

Install kubectl:
```
sudo apt-get install -y kubectl
```

Install AWS CLI:
```
sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

Install the unzip and python on Ubuntu if not installed
```
 sudo apt install unzip python
```

Extract the aws cli bundle setup
```
 sudo unzip unzip awscliv2.zip
```

Configure the AWS CLI on Ubuntu
```
sudo ./aws/install
```

Verify the AWS CLI version
```
aws --version
```

### Create IAM User with full S3, EC2, Route53 and VPC access in AWS

The creation can be done using AWS GUI Console at "Identity and Access Management" -> "User" -> "Permission". The needed permissions are:

1. AmazonS3FullAccess 
2. AmazonEC2FullAccess 
3. AmazonRoute53FullAccess 
4. IAMFullAccess 
5. AmazonVPCFullAccess

### Configure AWS CLI and create EKS clustes

To connect AWS using CLI we have configure AWS user using below command:
```
aws configure
```

And pass the informations needed to:
```
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: 
Default output format [None]: 
```

After CLI configuration use the follow command to create an EKS with 2 nodes at ap-south-1 region:
```
eksctl create cluster --name demo-ekscluster --region ap-south-1 --version 1.21 --nodegroup-name linux-nodes --node-type t2.micro --nodes 2
```


