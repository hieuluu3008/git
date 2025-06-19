# I. Introduction to Git

### Definition 
One popular program for version control - Git <br>
Git is open source and scalable to easily track everything from small solo projects to complex collaborative efforts with large teams!

* Version of Git is installed
```bash
git --version
```
* See our location
```bash
pwd
```
* List down all files in directory
```bash
ls
```
* Move to another directory
```bash
cd another_directory
```
* Create a new repo
```bash
git init new_repo
```
* Convert existing dir into a Git repo
```bash
git init
```

### Git workflow
STAGING AREA --> COMMIT AREA <br>
* Add to staging
```bash
git add README.md
```
Add single file.
```bash
git add . 
```
Add all files. 
* Check the current status
```bash
git status
```
* Make a commit
```bash
git commit -m "Adding files"
```

### Version control
#### Commit Structure
Git commits have three parts:
1. Commit
* contains the metadata - author, log message, commit time
2. Tree
* tracks the names and locations of files and directories when that commit happened
* like a dictionary - mapping keys to files or directories
3. Blob
* Binary Large Object
* may contain data of any kind
* a compressed snapshot of a file's contents
![Visualizing the commit structure](image/commit_structure.png)
Git hash: unique identifier .Hashes enable Git to share data efficiently between repos. If two files are the same, their hashes will be the same.
`b22eb75a82a68b9cOf1c45b9f5a9b7abe281683a`

* Shows commit from new to old
```bash
git log
```

#### Versions history
* Display the two most recent commits of a specific file
```bash
git log -2 current_file.sql
```
* Display by the date range
```bash
git log --since='Apr 2 2025' --until='Apr 11 2025'
```
* Show a particular commit
```bash
git show c25fa520
```

#### Compare versions
* Show difference between versions
```bash
git diff report.sql
```
* Compare latest commit ver vs staging ver
```bash
git diff --staged report.sql
```
* Compare two commit hashes
```bash
git diff 35af4a5 185a4h6
```
Show what change fomr 1st to 2nd hash.
* Use HEAD in compare
```bash
git diff HEAD~1 HEAD
```

#### Restore and revert files
1. Revert
Restoring a repo to the state prior to the previous commit
* Reinstates previous versions and makes a commit
* Restores all files updated in the given commit
```bash
git revert HEAD
```
* Avoid open the text editor
```bash
git revert --no-edit HEAD
```
* Revert without commit (bring files into staging)
```bash
git revert -n HEAD
```
* Revert a single file
```bash
git checkout 35af4a5 -- report.md
```
* Check the checkout
```bash
git status
```
2. Restore
* Restore a single file (unstage a single file)
```bash
git restore --staged summary.sql
```
* Restore all files
```bash
get restore --staged
```

### Branch
An independent line of development that splits off from the main codebase (usually the `main` or `master` branch).
* List all branches
```bash
git branch
```
record with `*` is noted for current branch

#### Create branch
* Switch between branches
```bash
git switch main
# or
git checkout main
```
* Create a new branch
```bash
git branch order
```
* Create and switch to it
```bash
git switch -c order
# or
git checkout -b order
```

#### Modify & Compare branches
* Compare two branches
```bash
git diff main order
```
* Rename a branch
```bash
git branch -m order purchase_order
```
Change the name of branch from 'order' to 'purchase_order'
* Delete a branch
```bash
git branch -d purchase_order
```
If you have not merged the branch to `main`
```bash
git branch -D purchase_order
```

#### Merge branch
Source and destination
* the last commits from each branch are called parent commits
* source -- the branch we want to merge from
* destination -- the branch we want to merge into a.k.a `main`
```bash
git merge order
```
#### Merge conflict
* Open a file
```bash
nano order
```



### Remotes
Remote repo - a repo stored in the cloud through an online repo hosting service such as GitHub.
* Clone a repo
```bash
git clone path-to-project-repo
```
* Clone a remote
```bash
git clone https://github.com/hieult/project
```
* List all remotes associated with the repo
```bash
git remote -v
```
* Add remote by a name
```bash
git remote add remote_name https://github.com/hieult/project
```
* Fetch from a remote
```bash
git fetch origin
```
- Fetch all remote branches
- Create new local branches if they only existed in the remote
- Doesn't merge the remote's contents into local repo
* Fetch only from the `origin` remote's `main` branch
```bash
git fetch origin main
```
* Merge origin remote's default branch - `main` into the local repo's current branch
```bash
git merge origin
```
* Simplifies process
```bash
git pull origin 
```
* Pull a certain branch
```bash
git pull origin dev_branch
```
* Push into `remote` from `local_branch`
```bash
git push remote local_branch
```
* Push changes into `origin` from local `main` branch
```bash
git push origin main
```
### Merge
#### Merge types
1. Fast forward merge:
A fast-forward merge is a type of merge in Git that occurs when the current branch has no new commits since it diverged from the branch being merged. In this case, Git does not create a new merge commit — it simply moves the pointer of the current branch forward to match the other branch.
```bash
git checkout main
git merge feature_branch
```
Forge git merge fast forward
```bash
git checkout main
git merge --ff-only feature_branch
```
- Before
```css
A---B---C (main)
         \
          D---E (feature)
```
- After
```css
A---B---C---D---E (main, feature)
```

2. Recursive merge:
- Recursive merge is the default merge strategy in Git, used when two branches have made separate commits after diverging from a common ancestor.
- When you merge two branches that cannot be fast-forwarded, Git uses the recursive strategy to automatically analyze and combine changes from both branches.
```bash
git checkout main
git merge --no-ff feature_branch
```
- Before
```css
       D---E (feature)
      /
A---B---C (main)
```
- After
```css
       D---E
      /     \
A---B---C-----M (main)
```

3. Squash merge:
- Creates a single new commit on the target branch
- Combines all changes from the source branch. Add a regular commit with one parent
- Added to target branch (not feature branch). Doesn't preserve the detailed commit history of the source branch
```bash
git checkout main
git merge --squash feature
git commit -m "Add new feature"
```
- Before
```css
A---B---C (main)
         \
          D---E---F (feature)
```
- After
```css
A---B---C---G (main)
         \
          D---E---F (feature)
```
G = D -> F

4. OCtopus merge
- Merge three or more branches at once
- Create a single merge commit with multiple parents
- Best used when branch don't conflict with each other
```bash
git checkout main
git merge -s octopus feature1 feature2 feature3
```
- Before
```css
A---B (main)
 \
  C (feature1)
   \
    D (feature2)
     \
      E (feature3)
```
- After
```css
A---B------------M (main)
 \   \    \     /
  C   D    E--- (feature1, feature2, feature3)
```
#### Rebase
```bash
git rebase <branch_name>
```
A command in Git used to move or copy an entire sequence of commits from one branch onto the tip of another branch, as if those commits were originally made from the new base.
- Before
```css
A---B---C---F (main)
         \
          D---E (feature)
```
- Run rebase
```bash
git checkout feature
git rebase main
```
- After
```css
A---B---C---F---D'---E' (feature)
```
* Interactive rebase
Edit multiple commits in history at one time
```bash
git rebase -i <commit_hash>
```
- Before
```bash
$ git log --online data-validation
abc1234 Optimize validation performance
def5678 Fix validation bug
ghi9101 Add data validation function
xyz1234 New Main Branch Commit
VWX7890 Main Branch Commit
jk12345 Initial commit
```
- Modify last 3 commits
```bash
git rebase -i HEAD~3
```
- Squash these three commits into one commit using the base cmd `pick` and `fixup`
- - `pick` keep the commit
- - `fixup` combine the commit with prev pick commit
```bash
$ git rebase -i HEAD~3
pick ghi9101 Add data validation function
fixup def5678 Fix validation bug
fixup abc1234 Optimize validation performance
```
- After
```bash
$ git log --online data-validation
mno6789 Add data validation function
xyz1234 New Main Branch Commit
VWX7890 Main Branch Commit
jk12345 Initial commit
```
### Cherry-Pick
A command that copies a specific commit from one branch and applies it to another.
* Single Commit
```bash
git cherry-pick <commit-hash>
```
Example: <br>
Before
```css
A---B---C---D---E  (main)
         \
          F---G   (feature)
```
Cherry-pick
```bash
git checkout main
git cherry-pick G
```
After
```css
A---B---C---D---E---G'  (main)
         \
          F---G         (feature)
```
* Multiple Commits
```bash
git cherry-pick <hash1> <hash2>
```
* With range
```bash
git cherry-pick <hash1>...<hash4>
```
#### Resolve cherry-pick conflict
* Conflict resolution steps
1. Manually edit conflict files
2. Add resolved file to staging using
```bash
git add <resolved file>
```
3. Continue with the cherry-pick process by running --continue flag
```bash
git cherry-pick --continue
```
* Stop a cherry-pick
```bash
git cherry-pick --abort
```

### Bisect
A tool that uses binary search to find the commit that introduced a bug.
```bash
git bisect
```
#### How to start
Ex: You’ve discovered a bug in the current `HEAD` commit, but everything was working fine 10 days ago in commit `abc1234`.
```bash
git bisect start               # star bisect process
git bisect bad                 # mark the current HEAD is bad (error)
git bisect good abc1234        # mark the commit abc1234 is good (normal)
```
1. Git checks out a commit halfway between the specified good and bad commits
2. Use testing process to verify whether a specific version is working as expected or if a bug exists
3. 1. When the test fails and the bug exists, run `git bisect bad`. 
3. 2. When the test passes, run `git bisect good`.
4. Git selects the next commit to test until it finds the closest commit that introduced the bug, run `git bisect reset`

#### Automate test
```bash
git bisect start
git bisect bad
git bisect good abc1234
git bisect run ./test-script.sh
```
Trong đó `test-script.sh` là file shell script trả về:
- Exit code `0` nếu pass
- Exit code khác `0` nếu fail

### Git Filter Repo
A tool for rewriting Git repository history quickly and safely
```bash
git filter-repo
```
#### Filter-Repo process
1. Install git filter-repo
```bash
pip install git-filter-repo
```
2. Remove secrets.txt from every commit
```bash
git filter-repo --path secrets.txt --invert-paths --force
/*
--path: speccifies which paths to operate on
--invert-paths: operate on all paths except the ones specified in --path
*/
```

### Git reflog
- A log of all reference updates in our local repository.
- A git reference can be a pointer to a specific commit, a branch, a tag, or the HEAD pointer. 
- The reflog data is stored in the .git/logs/refs/heads/ directory.

| Feature         | `git reflog`                                          | `git log`                                                  |
| ----------------| ----------------------------------------------------- | ---------------------------------------------------------- |
| Purpose         | Shows reference updates and rewrites in local repo    | Shows commit history only                                  |
| Scope           | Local repo only                                       | Local and remote repo                                      |
| Content         | All ref updates (commits, resets, merges, etc.)       | Only commits                                               |
| Persistence     | Temporary (usually 90 days)                           | Permanent (part of repository history)                     |
| Use Case        | Recovering lost commits, understanding recent actions | Viewing project history, understanding feature development |            

* Display log data and HEAD activity
```bash
git reflog
git reflog show
```
* Clean up old log or unreachable entries
```bash
git reflog expire
```
* Filtering
```bash
git reflog --since/until= "1 week ago"
```
* Recover deleted branch
```sql
git reflog -- Identify the hash
git checkout <hash> -- move HEAD to the hash of delete branch
git checkout -b <branch-name> -- Create a new branch
```

### Git reset
| Reset Type      | Command                        | Effect on Working Directory      | Effect on Staging Area    |
| ----------------| ------------------------------ | -------------------------------- | ------------------------- |
| Soft            | `git reset --soft <commit>`    | No changes                       | Changes remain staged     |
| Mixed (Default) | `git reset --mixed <commit>`   | No changes                       | Changes are unstaged      |
| Hard            | `git reset --hard <commit>`    | Changes are discarded            | Changes are discarded     |

### Git Worktrees
```bash
git worktree
```
- "checkout" multiple branches in your workspace.
- Similar to a repo checkout, but efficient.
- No need for stashing changes.
- No need to switch between branches during development.

#### Git Worktree vs Git Switch
| Git Worktree               | Git Switch                     | 
| ---------------------------| ------------------------------ |
| Multiple active branches   | Only active branch at a time   |
| Separate directories       | Single working directory       | 
| No need to stash changes   | May require stashing           |

#### Create a Git Worktree
```bash
git worktree add <path> <branch>
```

List all active worktrees
```bash
git worktree list
```
Remove a worktree
```bash
git worktree remove <path>
```

### Git Submodules
A Git repo nested inside another repo  
Separate version control and history  
Submodule change does not affect main repo  


