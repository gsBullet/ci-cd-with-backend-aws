# how to add github ci/cd in aws ec2, i want to keep my bakend in ec2


# 1.SSH into your EC2
    ssh -i your-key.pem ubuntu@your-ec2-ip
# 2. Install required dependencies (Node.js, PM2, Git, etc.)
    sudo apt update && sudo apt upgrade -y
    sudo apt install git -y
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt install -y nodejs
    sudo npm install -g pm2
# 3. Clone your repo for the first time
    cd /var/www
    git clone https://github.com/your-username/your-backend.git
    cd your-backend
    npm install
    pm2 start server.js --name backend
    pm2 save

# Configure SSH Access for GitHub Actions
<p>
  Your GitHub Actions workflow needs a way to connect to EC2 securely. <br>
  On your local machine, generate a new SSH key (don’t overwrite your existing one):
</p>

