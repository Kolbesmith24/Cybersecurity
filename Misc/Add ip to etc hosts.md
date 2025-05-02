# Cant open box / website
- add website to your `/etc/hosts` file
```
sudo echo "10.10.18.159 www.smol.thm smol.thm" | sudo tee -a /etc/hosts
```
# WordPress sites
## Scan for more info on wordpress sites:
```
wpscan --url http://www.smol.thm
```
# Environment Setup
The provided script required specific Python packages, so I created a **virtual environment** to run it cleanly:
1. Create a virtual environment
```bash
python3 -m venv ~/myenv
```
2. Activate the environment
```
source ~/myenv/bin/activate
```
3. Install dependencies
```
pip install pycryptodome sympy
