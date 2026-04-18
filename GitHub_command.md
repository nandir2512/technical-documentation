# Mac User
## Git hub Connection from Local

### Step 1: Check if SSH key already exists
  Run: ```ls -al ~/.ssh```
  
  Look for files like:
  
    id_ed25519
    id_ed25519.pub
    
  If they exist, you may already have a key.
    
### Step 2: Create SSH Key
  If no key exists, run:
  ``` ssh-keygen -t ed25519 -C "your_email@example.com" ```

### Step 3: Start SSH Agent
  Run: ```eval "$(ssh-agent -s)" ```
  
Then:
  ```ssh-add --apple-use-keychain ~/.ssh/id_ed25519```
  
If that gives error, use:
  ```ssh-add ~/.ssh/id_ed25519```

### Step 4: Copy Public Key
  Run: ```pbcopy < ~/.ssh/id_ed25519.pub```

### Step 5: Add Key to GitHub
  1. Go to Github setting page
  2. Then: Click New SSH Key
  3. Title: <MacBook>
  4. Paste the copied key
  5. Click Add SSH Key

### Step 6: Test Connection
  Run: ```ssh -T git@github.com```

  You should see something like:
  
    Hi nandir2512! You've successfully authenticated
  
