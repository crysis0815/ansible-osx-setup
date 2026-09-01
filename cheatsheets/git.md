# KnowledgeBase / Cheatsheet for git

## archive a git repository

To create a full, standalone archive of a repository (including all branches, tags, commit history, and remote ref headers), use a mirror clone.  
```
git clone --mirror https://github.com/username/repository.git repository-backup.git
tar -czvf repository-backup.tar.gz repository-backup.git
```  

**To restore:** Uncompress the file, navigate into the directory, and run git push --mirror

