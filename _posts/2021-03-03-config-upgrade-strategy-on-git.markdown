---
title: Config Upgrade Strategy on Git
author: Raymond
layout: post
date: 2021-03-03T19:45:25-05:00
categories:
  - Tech
---

I've been trying to figure out a config upgrade strategy with Git. I goolgled it but couldn't find any sound solution, which is hard to believe.

The problem: Our configuration files are a bunch of text files with some small binary images in a directory tree. Once we have a deployment, the config files will be changed muliple times depending on the needs. When a new software version is released, this deployment needs to upgrade to that version. For the config files this means mergeing the changes between the two release versions to the current deployment, which already contains some changes since it's first configured and deployed.

My solution:
Defins three types of branches.
1. Build Branch. This simply contains the default values of all the config files. E.g. build-v1, build-v2, build-v3
2. Deployment Branch. When we have a new deployment, we always start from a build branch, we always know which version we are deploying, right? Then we give the deployment a name, this name is also the deployment branch name. Then we push whatever changes we need to the deployment branch, and only deploy the files on this branch to the runtime environment. So it's basically:
```
  git checkout build-v2
  git checkout -b mydeploy1
  ... make changes
  git add -A
  git commit -m "Some changes"
  git push
```
3. Upgrade Candidate Branch. Once we need to upgrade e.g. to v3, we create a upgrade candidate branch based on the latest deployment branch. Then we use git diff to calculate the delta between build-v2 and build-v3, and play the delta on the upgrade candidate to perform a 3-way merge. Commands:
```
  git checkout mydeploy1
  git pull --ff-only
  git checkout -b upgrade-candidate-v2-v3
  // Assuming b6b3d0 is the hash of build-v2, 9af1e8 is hte hash of build-v3
  git diff --binary b6b3d0..9af1e8 | git apply -3
  git add -A
  git commit -m "Upgrade candidate v2-v3"
  git push
```
Now the 'upgrade-candidate' branch has the merge of the latest 'mydeploy1' depolyment branch merged with the delta between the default build-v2 and build-v3. 
We should do a manual review. This manual step can not be skipped because it's a 3-way merge. There is always a chance of conflict. We have to resolve all the conflicts, push new changes to the upgrade candidate branch. When we're happy, we can create a PR for merging it to the deplyment branch 'mydeploy1'. Once the merge is finished, the config upgrade is done.

