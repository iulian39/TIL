# TIL

## GIT

git checkout -b NEWBRANCH
git push --set-upstream origin BRANCHNAME

git pull origin your_branch # fetch and merge in one operation

git stash
git stash pop

### Ammend
git commit --amend 
:wq
git push -f ...  

### Change remote link 
git remote -v
git remote set-url origin NEW_URL
git pull # to verify

## Docker
### Starting a docker-compose.yml file
docker-compose pull
docker-compose up -d

### Building a docker image
docker build . -t NAME
docker run -p 8070:8080 NAME(Might also be a  url)
Then you can see it on  http://localhost:8070/Index.html
