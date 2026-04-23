# Steps for Python Install
## Linux 



# Create Python virtual environments
## Using `UV`

'''
  $ pip install uv
  $ uv venv --python 3.13
  $ source .venv/bin/activate
  $ python --version
    Python 3.13.13
  $ uv pip install ipykernel
  $ python -m ipykernel install --user --name uv-py313 --display-name "Python3.13 (uv)" 
  $ uv pip install numpy==2.4.4 pandas==3.0.2 ipywidgets
'''
