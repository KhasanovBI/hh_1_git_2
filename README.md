```bash
# 1. Должна содержать только frontik/testing.
git clone https://github.com/hhru/frontik frontik_testing
cd frontik_testing
for branch in $(git branch --all | grep '^\s*remotes' | egrep --invert-match '(:?HEAD|master)$'); do
    git branch --track "${branch##*/}" "$branch"
done
git filter-branch --prune-empty --subdirectory-filter frontik/testing  --tag-name-filter cat -- --all 
git remote set-url origin https://github.com/KhasanovBI/frontik_testing
git push origin --mirror
# 2. Должна содержать всё кроме frontik/testing.
cd ..
git clone https://github.com/hhru/frontik frontik_no_testing
cd frontik_no_testing
for branch in $(git branch --all | grep '^\s*remotes' | egrep --invert-match '(:?HEAD|master)$'); do
    git branch --track "${branch##*/}" "$branch"
done
git filter-branch --index-filter 'git rm -r --cached --ignore-unmatch frontik/testing' --prune-empty --tag-name-filter cat -- --all
git remote set-url origin https://github.com/KhasanovBI/frontik_no_testing
git push origin --mirror
```
