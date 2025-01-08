## How to unstage changes that are staged ..  
```bash
   git rm --cached <your file name > or .
   git restore --staged <your file name> or .  
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
senirio -> let say you are working on the project and make two file a.txt and b.txt now you did the commit . Now due to some reason   
you have to create new file named c.txt and this file you want to add on the previous commit rather then making a new commit . 
so you can do that user option  " --amend" , this option enable you add your changes on the last commit. 

Example . 
` git commit -m "Updated configuration " --amend `

