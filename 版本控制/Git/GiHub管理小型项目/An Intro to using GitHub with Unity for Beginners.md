---
tags:
  - GitHub
  - 版本控制
---
# 链接

https://github.com/orgs/community/discussions/56071

---

# 操作流程

## 1. Install git and create a GitHub account

To begin, you’ll need to install GitHub Desktop, which you can download from the official GitHub website ([https://desktop.github.com/](https://desktop.github.com/)). Once installed, create a GitHub account if you don’t already have one. Sign in to the GitHub Desktop app using your credentials.

用GitHub Desktop，方便些。

## 2. Setup Git

Before integrating GitHub Desktop with your Unity project, ensure that you have Git installed on your machine. If not, download and install Git from the official website ([https://git-scm.com/](https://git-scm.com/)).

## 3. Create a local Git repository

1. Create a New Repository: In GitHub Desktop, click on the “File” menu and select “New Repository” or click on the “Create a new repository on GitHub” button on the home screen.

2. Fill in Repository Details: In the “Create a New Repository” dialog, provide the necessary information for your Unity project:

- Name: Enter a name for your repository. This should be a unique and descriptive name.
- Description: Optionally, provide a brief description of your project.
- Local Path: Choose the local directory where you want to create the repository. This should be the root directory of your Unity project.

1. Initialize Repository: Check the “Initialize this repository with a README” option if you want to create an initial README file for your repository. This is helpful for providing project documentation.

2. Add .gitignore: Select the appropriate .gitignore template for Unity projects. You can search for “Unity” in the provided input field to find the relevant options.It’s essential to include a .gitignore file to exclude unnecessary files from version control.

Finally, Click on the “Create Repository” button to create the new repository on GitHub and initialize it with the provided settings.

创建项目，用 Unity的ignore模板。

## 5. Setup Git LFS

1. Install Git LFS from the [official website](https://git-lfs.com/).
2. Open a terminal or command prompt in your repository’s root directory.
3. Run git lfs install to initialize Git LFS.
4. Specify the file types you want to track in the .gitattributes file using git lfs track "*.extension".

gitattributes Example:

```
## git-lfs ##

#Image
*.jpg               filter=lfs diff=lfs merge=lfs -text
*.jpeg              filter=lfs diff=lfs merge=lfs -text
*.png               filter=lfs diff=lfs merge=lfs -text
*.gif               filter=lfs diff=lfs merge=lfs -text
*.psd               filter=lfs diff=lfs merge=lfs -text
*.ai                filter=lfs diff=lfs merge=lfs -text

#Audio
*.mp3               filter=lfs diff=lfs merge=lfs -text
*.wav               filter=lfs diff=lfs merge=lfs -text
*.ogg               filter=lfs diff=lfs merge=lfs -text

#Video
*.mp4               filter=lfs diff=lfs merge=lfs -text
*.mov               filter=lfs diff=lfs merge=lfs -text

#3D Object
*.FBX               filter=lfs diff=lfs merge=lfs -text
*.fbx               filter=lfs diff=lfs merge=lfs -text
*.blend             filter=lfs diff=lfs merge=lfs -text
*.obj               filter=lfs diff=lfs merge=lfs -text

#ETC
*.a                 filter=lfs diff=lfs merge=lfs -text
*.exr               filter=lfs diff=lfs merge=lfs -text
*.tga               filter=lfs diff=lfs merge=lfs -text
*.pdf               filter=lfs diff=lfs merge=lfs -text
*.zip               filter=lfs diff=lfs merge=lfs -text
*.dll               filter=lfs diff=lfs merge=lfs -text
*.unitypackage      filter=lfs diff=lfs merge=lfs -text
*.aif               filter=lfs diff=lfs merge=lfs -text
*.ttf               filter=lfs diff=lfs merge=lfs -text
*.rns               filter=lfs diff=lfs merge=lfs -text
*.reason            filter=lfs diff=lfs merge=lfs -text
*.lxo               filter=lfs diff=lfs merge=lfs -text
```


## 6. Create a new branch

Branches allow you to work on different features or bug fixes independently. To create a new branch in GitHub Desktop, click on the “Current branch” dropdown and select “New branch.” Provide a descriptive name for the branch that reflects the work you’ll be doing.

## 7. Add a new file to the repo

GitHub Desktop automatically tracks any changes made to files within the repository. To add a new file to your Unity project, simply create or import it into the project folder on your local machine. GitHub Desktop will detect the addition and display the file as an untracked change.

## 8. Create a commit

A commit represents a snapshot of your project at a specific point in time. To commit changes, review the untracked file(s) in the “Changes” tab of GitHub Desktop. Select the file(s) you want to include in the commit, provide a meaningful commit message, and click on the “Commit to [branch name]” button. This action commits the changes to your local repository.

## 9. Push a branch to GitHub

When you’re ready to merge your changes into the main branch, you’ll need to create a pull request. On the GitHub website, navigate to your repository and click on the “New pull request” button. Select the appropriate branches to compare, review the changes, and provide a descriptive title and description for your pull request.

## 11. Merge a PR

When you’re ready to merge your changes into the main branch, you’ll go through the process of reviewing your PR. Someone from your team should review the changes, leave comments, start a review, you can then resolve the discussions, and merge the PR using appropriate options, and optionally delete the branch when your done.

## 12. Merge Conflicts

Merge conflicts occur when there are conflicting changes between branches. GitHub Desktop provides a visual interface to resolve conflicts. After attempting to merge branches, if a conflict arises, GitHub Desktop will indicate the files with conflicts. Click on the conflicted file(s), resolve the conflicts manually, and save the changes. Finally, commit the resolved conflicts to complete the merge.

Handling merge conflicts with Unity’s binary scene and prefab files can be challenging. To mitigate conflicts, communicate and coordinate changes with team members. Break complex scenes and prefabs into smaller components, use prefab variants, avoid deep hierarchies, and consider source control locking for critical files. Clear documentation and communication are key during conflict resolution, where manual merging may be required.

## Conclusion

We covered the essential steps, from installation to handling merge conflicts. Start harnessing the power of version control and take your Unity projects to new heights with GitHub Desktop!