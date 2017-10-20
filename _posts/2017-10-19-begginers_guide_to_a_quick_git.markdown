---
layout: post
title:      "Begginers Guide to a Quick Git"
date:       2017-10-20 00:43:01 +0000
permalink:  begginers_guide_to_a_quick_git
---


Git is an awesome tool every developer should use. However getting started can be a bit confusing. I made myself a cheat sheet that really helped me learn the basics, and I ended up referencing it a lot more than I initially thought.

Keep in mind these are just the basics. If you run into an issue, try looking up the proper git terminology for what you want to do, and just search for that. Nothing is more than a command line away. Now git coding! hahaha. Git it?!?

Git commands:

Forking:
1. Fork a project on Github
2. Copy the SSH code
3. in terminal: cd <to-whatever-file-you-want-to-save-in>
4. git clone <SSH-link-from-above>
5. cd <name-of-lab-from-SSH-link>
6. git checkout -b <name-the-branch-here>
8. git status (check status) -- optional (make sure you are on the branch you just created)
8. sublime . (or whatever command you setup to open your text editor)

Pushing:
1. in terminal: git status -- optional: again just checking the branch status
2. git add . (use " . " for all files changed, or point to indivisual file)  
3. git commit -m “add your upload comment here”
4. git push -u origin <branch-name-here>
  
Pushing to master branch:
1. in terminal: git checkout master
2. git merge <branch-name-here>
3. git push origin master

Submit pull request
1. open recent repos in GitHub account
2. click “new pull request”
3. name request “Done” or whatever you want.
4. add to master branch in learn-co-students
5. submit pull request

