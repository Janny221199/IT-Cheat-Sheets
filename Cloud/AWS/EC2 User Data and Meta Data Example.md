
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd  

META_DATA_ENDPOINT="http://169.254.169.254/latest/meta-data"

# Get instance token
TOKEN=$(curl -X PUT -H "X-aws-ec2-metadata-token-ttl-seconds: 21600" -s http://169.254.169.254/latest/api/token)

# Get Mac (and also get first mac if you have multile)
MAC=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/network/interfaces/macs | head -n1 | tr -d '/')

# Get instance information using the token
VPC_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/network/interfaces/macs/${MAC}/vpc-id)
VPC_CIDR=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/network/interfaces/macs/${MAC}/vpc-ipv4-cidr-block)
AZ=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/placement/availability-zone)
REGION="`echo \"$AZ\" | sed 's/[a-z]$//'`"
SUBNET_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/network/interfaces/macs/${MAC}/subnet-id)
SUBNET_CIDR=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/network/interfaces/macs/${MAC}/subnet-ipv4-cidr-block)
PUBLIC_IP_RESPONSE=$(curl -o /dev/null -w "%{http_code}" -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/public-ipv4)
if [ "$PUBLIC_IP_RESPONSE" -eq 404 ]; then
	PUBLIC_IP="-"
else
	PUBLIC_IP=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4)
fi
PRIVATE_IP=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/local-ipv4)
INSTANCE_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s $META_DATA_ENDPOINT/instance-id)

# Create HTML content with instance information
cat <<EOF > /var/www/html/index.html
<h1>Instance Running properly!</h1>
<p>Region: $REGION</p>
<p>Selected Mac for VPC and Subnet: $MAC</p>
<p>VPC ID: $VPC_ID</p>
<p>VPC CIDR: $VPC_CIDR</p>
<p>Availability Zone: $AZ</p>
<p>Subnet ID: $SUBNET_ID</p>
<p>Subnet CIDR: $SUBNET_CIDR</p>
<p>Public IP: $PUBLIC_IP</p>
<p>Private IP: $PRIVATE_IP</p>
<p>Instance ID: $INSTANCE_ID</p>
EOF
```

