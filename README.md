# Handsfree-stable-diffusion for Stable Diffusion WebUI
Handsfree-stable-diffusion is a CloudFormation template plus script for deploying Stable Diffusion webui with some tweats. This script automates the prerequisites, installs sd-webui based on the amazing [`AUTOMATIC1111/stable-diffusion-webui`](https://github.com/AUTOMATIC1111/stable-diffusion-webui) and some tweaks that can help getting started with stable diffusion

This Cloudformation template is tested on [`G5 instance`](https://aws.amazon.com/ec2/instance-types/g5/) with `Deep Learning AMI GPU PyTorch 2.0.0 (Ubuntu 20.04) 20230401` at `us-east-1` region. If other regions are preferred, make sure G5 is available in the region and change to an equivalent AMI id for that particular region

# Features
- Launch an Ubuntu EC2 with Nvidia A10G GPU via CloudFormation
- Create an Elastic IP and attach to the EC2, this allow the EC2 to always use the same IP
- Installations
  - The prerequisites for stable diffusion webui
  - Stable diffusion webui (Automatic1111)
  - Installs tweaks (by default, but optional, [see the Tweaks section](#Tweaks))
- Launches Stable diffusion and `nvtop` with `tmux` for backend monitoring

# Prerequisites
- An AWS account
- `awscli`, see [official docs here](https://aws.amazon.com/cli/) 
- A key pair for EC2, i.e `keypair.pem`

# Usage 
1. Clone this repo and change directory to the project
```
git clone https://github.com/MarcoLeongDev/handsfree-stable-diffusion.git
cd handsfree-stable-diffusion
```
2. Copy and edit the shell command. Changes needed:
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

# Clean up remove CLI

** Caution the following wipe everythig **
```
aws cloudformation delete-stack --region us-east-1 --stack-name "my-stack-name"
```

# Tweaks
```
Lora       add_detail
Model      stable-diffusion-v2-1
VAE        EMA and MSE for fixing face/eyes
embeddings ng_deepnegative, verybadimagenegative, easynegative, badhand
Tools      ControlNet, openpose
Info       system info, images-browser
Theme      kitchen-theme
Safety     nsfw-censor (remove for the adventurous)
```


# FAQ
1. I am using another region, how do I find the right AMI image id?
2. How do I skip the tweaks and just use the Cloudformation template and install stable diffusion webui?
3. I see AGPL license, can I use the code for my business?
4. Can I use the creation of stable diffusion for my business?
5. I like ugly kangaroo, can I have more?
6. Do you only prompt-engineering ugly kangaroos? https://pixai.art/@marcoleong
7. Can I get you a coffee?
