# Create a new source code repository
#------------------------------------

# Clone the workshop repository locally
git clone https://gitlab.forge.orange-labs.fr/kermit/tutorials/workshop.git

# Remove kermit as remote URL and add your personal GitHub public account
cd workshop
git remote remove origin
# git remote add origin https://github.com/<account_name>/<repo_name>.git
git remote add origin https://github.com/mtail/kermit.v3.git

git push origin master
