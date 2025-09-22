# Option 1: Use npm directly in PM2
    pm2 start npm --name my-app -- run dev --watch
# Check status:
    pm2 list
    pm2 logs my-app
# Commit your changes
    git add server/app.js
    git commit -m "Save local changes before pulling"
    git pull --no-rebase  
