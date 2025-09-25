# 1. Update your system
    sudo apt update
    sudo apt upgrade -y
# 2. Install Redis
      sudo apt install redis-server -y
# 3. Check Redis status
    sudo systemctl status redis
# 4. Enable Redis to start on boot
    sudo systemctl enable redis
# 5. Test Redis
    redis-cli
<p>
  then type
    
       ping
</p>

# 6. Optional: Configure Redis (set password)
    sudo nano /etc/redis/redis.conf
<p>
  Find the line:
  
     requirepass foobared

  Save and exit (Ctrl+O, Enter, Ctrl+X). Then restart Redis:
  
    sudo systemctl restart redis
Test authentication:

    redis-cli
    auth YourStrongPassword
    ping

</p>
