# Steps for Python Install
## Linux 



# Create Python virtual environments
## Using `UV`

    $ pip install uv
    $ uv venv --python 3.13
    $ source .venv/bin/activate
    $ python --version
      Python 3.13.13
    $ uv pip install ipykernel
    $ python -m ipykernel install --user --name uv-py313 --display-name "Python3.13 (uv)" 
    $ uv pip install numpy==2.4.4 pandas==3.0.2 ipywidgets

## Using ` Conda / Miniconda / Anaconda`

    $ conda create -n myenv python=3.13
    $ conda activate myenv
        If Conda activation is not working then check 
            1. The issue is likely that conda isn't initialized for PowerShell. To fix that: Run : `conda init powershell`
            2. If Conda is already initialized. Then the issue might be PowerShell's execution policy blocking the conda hook script.  Run: `Get-ExecutionPolicy`
               output: Restricted
            If PowerShell's execution policy is **Restricted**, which blocks the conda hook script from running. Fix it with: Run: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
        Now let's activate the environment: `conda activate myenv`
       
    $python --version
    $python -m ipykernel install --user --name myenv
    $pip install -r requirements.txt

Other important command 

    $ conda info --envs
    
