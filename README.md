# assembly_programming_f

Add the upstream remote (the original repo you forked from)
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPO.git


Verify remotes
git remote -v


Fetching and merging changes
git pull upstream main (shortcut)

OR:
git checkout main
git merge upstream/main


Update local repo:
git push origin main



Always keep fork in sync:
git fetch upstream
git merge upstream/main
git push origin main
