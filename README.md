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
    
           ssh-keygen -t rsa -b 4096 -C "github-ci"
Save it as github-ci-key (private) and github-ci-key.pub (public).
</p> 

# 1. What is ~/.ssh/authorized_keys?
<p>
    On your EC2 instance, SSH checks a file called ~/.ssh/authorized_keys (inside the home folder of the user, e.g. ubuntu).
<br>
That file contains a list of public keys that are allowed to connect without typing a password.
check it your aws ubontu
    
        cat ~/.ssh/authorized_keys

<br>
you see like 

    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC4/GtIuHKFUBMCJbRTTQa.............../5MQ== github-ci
    
</p>

# 2. you generate a new SSH key pair on your local machine: (private key)

            cat ~/.ssh/github-ci-key
<p>
    you can see like this
    
            -----BEGIN OPENSSH PRIVATE KEY-----
        (lots of characters here)
        -----END OPENSSH PRIVATE KEY-----
*Copy the entire block, including BEGIN and END.*
</p>

# 3. you generate a new SSH key pair on your local machine: (Public  key)
        cat github-ci-key.pub
<p>
    you will see like this

    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC4/GtIuHKFUBMCJbRTTQabEQC7ez6J... github-ci
</p>

# 4. Permissions must be correct in your ASW ubuntu
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
# 5. Add your new key to the correct user: github-ci-key.pub
        echo "paste-your-github-ci-key.pub-content-here" >> ~/.ssh/authorized_keys
# 6. Again permission 
         chmod 700 ~/.ssh
        chmod 600 ~/.ssh/authorized_keys

# 7. Test again from your local machine: your aws public IP
        ssh -i github-ci-key ubuntu@<IP>
<p>
    Perfect 🎉 That means your GitHub Actions private key (github-ci-key) and the public key on EC2 are now matched.
</p>

# ✅ Next Steps: Setup GitHub Actions
<p>
    Go to: GitHub → Your Repo → Settings → Secrets and variables → Actions → Repository secrets
<br>
    *EC2_SSH_KEY → contents of your github-ci-key (private key) <br>
(full block including -----BEGIN OPENSSH PRIVATE KEY----- … -----END OPENSSHPRIVATE KEY-----)
</p>
<p>
    *EC2_HOST → your EC2 public IP <br>
    *EC2_USER → usually ubuntu (or ec2-user for Amazon Linux)
</p>





