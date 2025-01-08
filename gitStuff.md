## How to unstage changes that are staged ..  
```bash
   git rm --cached <your file name > or .
   git restore --staged <your file name> or .
   git reset < your file name > or .
```
## How to revert changes on a particular file for group of file . 
```bash
   git checkout -- < your file name > OR git chekcout -- .
```
## How to set your email id and user id on global and particular repo .  

Global setting
```bash
  git config --global user.name "your name"
  git config --globol user.email "your email "
```
Local setting 
```bash
  git config user.name "your name"
  git config user.email "your email "
```
## How to get user.name and user.email for your git project.  

Global setting
```bash
  git config --global user.name
  git config --global user.email 
```
Local setting
```bash
  git config user.name  OR git config --get user.name  
  git config user.email OR git config --get user.email  
```
## How to add new changes to your exiting commit . 
senirio -> let say you are working on the project and make two file a.txt and b.txt now you did the commit . Now due to some reason you have to create new file named c.txt and this file you want to add on the previous commit rather then making a new commit . 
so you can do that user option  " --amend" , this option enable you add your changes on the last commit. 

Example . 
` git commit -m "Updated configuration " --amend `

## How to update your last commit message ...  
You can use " --amend " that allow you to edit your commit message .  This will open a default editor and then you can edit and save the commit message .  
` git commit --amend `

## How to move your HEAD to particular commit . 
` git checkout < commit hash > ` 

## How to restore particular commit changes .  
Let say you have made 3 commit commit-1 , commit-2 & commit-3 in sequence order now let say you have make some changes on the file named "readme.txt" on commit-1 and then on the commit 2 but now you feel that on the commit-4 only chnages that are made on the commit-1 is enough so now you want that file as it on the commit-4. 

```bash
git log --all --graph  // this will list all the commit with commit hash in graph formate.
// Now pull that particular change from the commit .
git checkout adfaljadfakjdfieadaf first.txt
```

## How i can setup aliase for git commands .
This can be done by `git config --global alias.<--> "Your command not git " ` or `git config alias.<---> "your command not git "`

` git config --global alias.s "status" `

## How to remove git from your repository or project . 
This can be done by deleting .git folder on your project , you can see that folder by command `ls --all `
so command will be 
` rm -rf .git`

