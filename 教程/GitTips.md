#git总结

| U       |  S   |  C |
| :--------:   | :-----:   | :----: |
| 工作区Unstaged/红色       | 暂存区Staged/绿色     |   Commited   


---
- git checkout fname //撤销更改，避免报红 

---
- git add * //U-S
		- git add -A//手动删除文件之后，避免报红
		- git reset head //抵消git add 
		- 
	
---

- git commit -a -m "注释"//S-C
	- git reset --soft cid//将C中内容回退到S，恢复绿色
	- git reset --hard cid//丢弃C中内容
	- git revert cid //创建一次新的commit,覆盖当前
	

---
# 撤销操作
- 如果你还没stage，处于U
	- git checkout fname
- 如果你已经staged，处于S
	- git reset head
	- 再git checkout fname
- 如果你已经commited 
	- git reset --soft    

---
#分支什么的
- git checkout -b bname //创建分支
- git checkout branch -d bname//删除分支
- git log bname ^ master//我有，但master没有的
- git merge master //站在bname把master拉到本分支
- 分支上交内容给master 
	- git checkout master//S的内容跟着走的噢
	- git merge bname
- 如果master已更新两次，[u1,u2],bname只想要u1这一次merge到本地
	- git cherry-pick u1-cid
 
---

- .gitignore仅对U中的内容有效
	- *.class; /文件夹;
- git log//找cid，然后按q返回

---






 

