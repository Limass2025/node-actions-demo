

# Step-by-Step Guide: Creating a GitHub Actions Workflow from Scratch

## Prerequisites Setup
1. **Create a GitHub Account**
   - Go to [github.com](https://github.com) and sign up
   - Verify your email address

2. **Install Git**
   - Download from [git-scm.com](https://git-scm.com/downloads)
   - Verify installation: `git --version`

3. **Install Node.js and npm**
   - Download from [nodejs.org](https://nodejs.org)
   - Verify installation:
     ```bash
     node -v
     npm -v
     ```

## Step 1: Create a New Project Repository
1. Go to GitHub and create a new repository named "node-actions-demo"
2. Clone it locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/node-actions-demo.git
   cd node-actions-demo
   ```
![alt text](<Screenshot 2025-09-16 081538.png>)
![alt text](<Screenshot 2025-09-16 082240.png>)

## Step 2: Initialize a Node.js Project
1. Create a package.json file:
   ```bash
   npm init -y
   ```

2. Install Jest for testing:
   ```bash
   npm install --save-dev jest
   ```
![alt text](<Screenshot 2025-09-16 082350.png>)
3. Update package.json scripts:
   ```json
   "scripts": {
     "build": "echo 'Building project...' && mkdir -p dist && cp *.js dist/",
     "test": "jest"
   }
   ```

## Step 3: Create Project Files
1. Create `index.js`:
   ```javascript
   function add(a, b) {
     return a + b;
   }
   
   module.exports = add;
   ```
![alt text](<Screenshot 2025-09-16 082707.png>)

2. Create `index.test.js`:
   ```javascript
   const add = require('./index');
   
   test('adds 1 + 2 to equal 3', () => {
     expect(add(1, 2)).toBe(3);
   });
   ```
![alt text](<Screenshot 2025-09-16 082801.png>)

## Step 4: Create GitHub Actions Workflow
1. Create the workflow directory:
   ```bash
   mkdir -p .github/workflows
   ```

2. Create `main.yml`:
   ```bash
   touch .github/workflows/main.yml
   ```
![alt text](<Screenshot 2025-09-16 082913.png>)

![alt text](<Screenshot 2025-09-16 083042.png>)
## Step 5: Write the Basic Workflow
Add this content to `.github/workflows/main.yml`:
```yaml
name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
      
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        
    - name: Install dependencies
      run: npm install
      
    - name: Build project
      run: npm run build
      
    - name: Run tests
      run: npm test
```
![alt text](<Screenshot 2025-09-16 083155.png>)
## Step 6: Add Advanced Features
Update `.github/workflows/main.yml` to include:
```yaml
name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  NODE_ENV: production

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
      
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        
    - name: Install dependencies
      run: npm install
      
    - name: Build project
      run: npm run build
      
    - name: Run tests
      run: npm test
      
    - name: Use environment variable
      run: echo "Node environment: $NODE_ENV"
      
    - name: Conditional step
      if: github.event_name == 'push'
      run: echo "This step only runs on push events"
      
    - name: Set output
      id: set-output
      run: echo "build_status=success" >> $GITHUB_OUTPUT
      
    - name: Use output
      run: echo "Build status: ${{ steps.set-output.outputs.build_status }}"
```

## Step 7: Add Secrets Usage
1. In GitHub, go to your repository > Settings > Secrets and variables > Actions
2. Click "New repository secret"
3. Name: `API_TOKEN`
4. Value: `your-secret-token-here`
![alt text](<Screenshot 2025-09-16 083858.png>)
![alt text](<Screenshot 2025-09-16 083927.png>)

5. Update the workflow to use the secret:
```yaml
    - name: Use secret
      run: echo "API token is: ${{ secrets.API_TOKEN }}"
      if: github.event_name == 'push'
```
![alt text](<Screenshot 2025-09-16 084813.png>)
## Step 8: Commit and Push Your Changes
```bash
git add .
git commit -m "Add GitHub Actions workflow"
git push origin main
```

## Step 9: Verify the Workflow
1. Go to your GitHub repository
2. Click the "Actions" tab
3. You should see your workflow running
4. Click on the workflow to view the steps and logs
![alt text](<Screenshot 2025-09-16 085529.png>)
## Step 10: Troubleshooting Common Issues
1. **YAML Syntax Errors**:
   - Check indentation (use spaces, not tabs)
   - Validate with a [YAML linter](https://www.yamllint.com/)

2. **Permission Issues**:
   - Ensure repository has "Actions" permissions enabled
   - Go to Settings > Actions > General

3. **Secrets Not Available**:
   - Verify secret name matches exactly in workflow
   - Check secret is configured at repository level

4. **Node.js Version Issues**:
   - Ensure `node-version` matches your project requirements
   - Use a specific version (e.g., `18.x`) instead of `latest`

You now have a complete GitHub Actions workflow that:
- Triggers on push and pull requests
- Checks out code
- Sets up Node.js
- Installs dependencies
- Builds the project
- Runs tests
- Uses environment variables
- Implements conditional execution
- Shares data between steps
- Uses secrets securely# node-actions-demo