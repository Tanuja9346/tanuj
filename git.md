. What is the difference between git reset and git revert?
Answer:

git reset moves the branch pointer to a previous commit and can modify or delete history depending on the mode (--soft, --mixed, --hard).
0r 
it is used to move your header point to the specific commit, it modifies the commmit history by adjusting header point.

git reset commit-id 
git reset commit head~1()
git reset --soft commit-id (moved to stage )
git reset --hard commit-id (delete everything)
--mixed {unstage area}

git revert creates a new commit that undoes changes from a previous commit, preserving history. It's safe for shared branches.
or
and particular commitid what u have done changes that will remove and create new commit id and preserve history.changes removed and saved into new commitid its is main difference reset and revert u can use this in public repository collabrations

git revert commit-id
git revert head
git revert head~nummber or 2 etc
git revert commit-id --no-edit
git revert commit-id commit-id2 commit-id etc.

2. git stash and git cherry pick?
Git feature used to temporarily save your uncommitted changes (like modified or staged files) without committing them and we have to move other mode i.e any branch. It's like hitting "pause" on your work so you can come back to it later.

indeex stage is staging area.
it is stored stash list : stash {0}-->indicates latest stash no.
stash {1},stash {2}

git stash --> it will create one commit

u want get back stash content : git stash apply

git stash list --> list of stash

git stash apply --index 1 -->recent stash apply and list in stash list //u can apply on another branch also 

git stash save "message" --> give stash name.
git stash pop  -->apply and drop  the stash content and not in stash list
git stash drop
git stash show stashid -p
git stash branch branch-name stashid --> if u want add stash changes to seprate branch.

cherry-pick:
Copies a specific commit from another branch and applies it to your current branch.

git cherry-pick commit-id
git cherry-pick commit-id -e {can change message}

pick commit-id but keep in staging area not commiting:
git cherry-pick commit-id -n


Git help add
Git config --local user.name
Git config --list --show-origin
 Local, golbal/ user , system level configuration
 Git status
 Git init
 Working area to staging area :: git Diff
 Staging to repository area:: git diff -- staged
 Working area to repository area: git diff Head
 Untsage means git restore
 git diff means difference btwn a files
 git cat-file hashobj -p
 git rename: mv name actual name
 git mv name actual name :: change name
 git restore --staged filename:: moving staging area to unstage area i.e changes from latest commit
 git restore filename : u want to restore that unstaged files
 git restore --source head~2 index.txt--> also used to get particular commitid/changes of file
 git branching and switch to another branch
 .git--> objects--> hash objects and refs ---> all branches folder , config had all details of email and username.
 git branch -->list of branches
 git checkout branch name
 git branch branchname
git checkout -b branchname:: create and checkout
 rename branch:: git branch -m new-branchname
 delete branch: git branch -d branch-name
or -D


Merging of 2 branches:
fastforward and recurisive branch:
some commits in feature branch and no commits in master and now merge this feature branch into master it is called fast forward branch.
git checkout master
git merge featurebranchname

recurceive: do some commits in master branch and from there cretae another branch and checkout add files and commit and checkout to master and do merge so it will create new commit id.

merging conflicts:
same line different content while merging content to master.
u decide it and wt to keep and not to keep amd merge this.
git merge --abort


git rebase: alternative to merge option.doesn't peserve history and advance one and is used to cleanup your local commit.

git rebase branchname
 Take the commits from your current branch i.e master go to that branch and replay them on top of featurebranch.
 very imp:
"Take the commits from master and replay them on top of featurebranch."
master : a--b--c
feature: a--b--c--d
git checkout master
git rebase feature: a--b--c--d--a1--b1--c1



git log --oneline --graph

interactive rebase:

git checkout feature {uncessary/ rebase commitsbranch}
git rebase -i master(rebase into target branch) (remain this commit id and squash remaing ids which u took to squash and give single commit-id)  or remove unncessary commits and keep it clean so squash unncessary commitids and bring single commit id.

21. modify latest commit or last commit: i.e no need for extra commit
git commit -amend --> it will open file and add message and wq
git show hash1 -->compleate ndetails of commitid

22. cherry-pick: u want to  pick particular commit from one branch to another branch.and u dint want to merge all branch and pick particular commit and it is duplicate commits.

git cherry-pick commitid

23. detached head in git: git checkout commitid {so head is detached here ntg but master not here}
u can commit here but not comes under master branch.
if u want to handle those commits cretae feature branch and handle those detached head commits


24. git checkout: with branch name, commitid, head~2 or any number

git checkout head file.txt--> it will remove that particular file also it means undo changes/ revert file.

25. git switch branchname --> create to new branch and switch to git switch -c branchname but only uses branch name to switch.

26. git and github 
27. github repo clone for get repository from remote to local and pushing  changes to repo need permissions.github repo can access anyone if it is public.

28. github permissions: https and ssh
https: username and pswrd
ssh: genrate keys: ssh-keygen -f devops
add ssh key to ssh agent before check ssh agent running or not: eval `ssh-agent -s`
add privatekey to ssh agent: ssh-add keypath i.e ~/.ssh/keyname
ssh -T git@github.com: authenticated or not
add ssh key to github account

29. local repository associated with any remote repo to check: git remote -v

30. git remote add origin <url> --adding remote url
origin : short name to url u can also chane this name.
git remote rename origin tanuja-->rename

31. push all local commits to github repo: git push origin master

32. git push origin master--> first time local branch push to remote branch.
 u can push the data to not only one branch and also another branch bt :
 git push origin feature:master

33. git push -u origin master --> once u run this everytime u no need apply complete command to push changes just type:: git push it will automatically push to master

39. difference btwn main and master: both are same
git branch -M main: change branch 

checkout to remote tracking branches in the local repository
git branch -r : branches that are present in remote
git checkout origin/branch1 : u can go to that branch bcz when u clone remote repo to your local default branch only get remaining will not get but it is not in track

git switch - -->go back to branch

git switch branch name --> it will switch and it is track.

41. git fetch: download changes but not merge to local or working directory.
git fetch remote --> fetch all the branches
git fetch remote branchname --> fetch from particular branch.

git pull = git fetch + git merge

git pull origin master --> pull latest info from origin master branch nd merge those changes into current branch

github pages in github reposistory: create repo leela.githun.io
insert data and view in repo settings --> pages--> view url 

50. pr:
collabrators

51. git forking: there is no opermisssion needed.u can fork other reposistories.after u can also add pr in created fork repo to owner of that repo.

52. git tags: we can label commits by creating tag
lightweight tags, annotated tags.
it just a pointer to a commit, 

annoatated tags: extra metadata including email,author name, thae date and tagging message
git tag -a 1.1.0
git show 1.1.0

53. semantic versioning: 4.2.1 major.minor(new features).patch(bug fixes do not impact code how u run)
git tag --> list  of tags
git tag -l "*beta*" -->search wildcard tags
git checkout tag --> particulat tag
git tag 1.0.0

54. push tags to remote repo: 
git push origin 1.0.0 --> particulat tag
git push origin --tags --> push all tags

55. reflogs: just logs that git keeps us for as a recored what has we done
this keeps on your local activity not collabaraters
git reflog show main --> veiw logs for the tip of the main
u can also use number to see logs --> git reflog show main@{3}
git reflog show master@{1.day.ago}  --> git logs can see with time based qualifier

62. traversing reflog: 
wt eveer the things u deleted in reset u will get in reflog ::: lost commits get back
git reflog show master
git reset commitid {i.e deleted one} --hard

u cam get back commits only in 90 days prior only

64. alias names for frequently used commnads:
git config --show-origin --golabal --list 
in .gitconfig file just add [alias] section.
 s = status
 l = log
 cm = commit -m 
 br = branch