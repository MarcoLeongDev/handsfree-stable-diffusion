# Description
Handsfree-stable-diffusion is a CloudFormation template plus script for deploying Stable Diffusion webui with some tweats. This script automates the prerequisites, installs sd-webui based on the amazing [`AUTOMATIC1111/stable-diffusion-webui`](https://github.com/AUTOMATIC1111/stable-diffusion-webui) and some tweaks that can help getting started with stable diffusion

This Cloudformation template is tested on [`G5 instance`](https://aws.amazon.com/ec2/instance-types/g5/) with `Deep Learning AMI GPU PyTorch 2.0.0 (Ubuntu 20.04) 20230401` at `us-east-1` region. If other regions are preferred, make sure G5 is available in the region and change to an equivalent AMI id for that particular region

# Prerequisites
- An AWS account
- `awscli`, see [official docs here](https://aws.amazon.com/cli/) 
- A key pair for EC2, i.e `keypair.pem`

# Usage 
1. Clone this repo
```
git clone https://github.com/MarcoLeongDev/handsfree-stable-diffusion.git
```
2. Change directory to the project
```
cd handsfree-stable-diffusion
```
3. Copy and edit the shell command. Changes needed:
- `my-stack-name`: to a preferred name. This name is what you are going to see on CloudFormation console and to be referred to in awscli
- `keypair`: to the name of your key pair. e.g. `mykey.pem` should be `mykey`, without the `.pem` extension name

### MacOS CLI
```
STACK_NAME="my-stack-name"
KEY_NAME="keypair"
aws cloudformation deploy --template-file handsfree-sd-g5.yml --parameter-overrides "KeyName=$KEY_NAME"  --region us-east-1 --stack-name $STACK_NAME
READY_TIME=`date -u -v+30M`
INSTANCE_IP=`aws cloudformation describe-stacks --region us-east-1 --query 'Stacks[0].Outputs[0].OutputValue' --output text --stack-name $STACK_NAME` 
echo "\nHandsfree-sd script requires around 30 mins to complete stable diffusion + tweaks installation\n"
echo "Ready at around:   $READY_TIME"
echo "webui link:        http://"$INSTANCE_IP":7860"
echo "SSH to the EC2:    ssh -i \"$KEY_NAME.pem\" ubuntu@$INSTANCE_IP"
echo "\nYou can also monitor with the progress \`tmux a\` by ssh to the instance after a couple of minutes\n"
```

### Linux/Unix CLI
```
STACK_NAME="my-stack-name"
KEY_NAME="keypair"
aws cloudformation deploy --template-file handsfree-sd-g5.yml --parameter-overrides "KeyName=$KEY_NAME"  --region us-east-1 --stack-name $STACK_NAME
READY_TIME=`date -u --date="+30 minutes"`
INSTANCE_IP=`aws cloudformation describe-stacks --region us-east-1 --query 'Stacks[0].Outputs[0].OutputValue' --output text --stack-name $STACK_NAME` 
echo "\nHandsfree-sd script requires around 30 mins to complete stable diffusion + tweaks installation\n"
echo "Ready at around:   $READY_TIME"
echo "webui link:        http://"$INSTANCE_IP":7860"
echo "SSH to the EC2:    ssh -i \"$KEY_NAME.pem\" ubuntu@$INSTANCE_IP"
echo "\nYou can also monitor with the progress \`tmux a\` by ssh to the instance after a couple of minutes\n"
```
### Windows Command Prompt / Powershell
```
Kindly provide some suggestions for Windows
```


