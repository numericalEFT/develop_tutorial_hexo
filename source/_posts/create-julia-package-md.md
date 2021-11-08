---
title:  "Create new julia package with PkgTemplate.jl"
date: 2021-08-21 21:37:48
tags: julia
---

Steps to create a julia package:

1.  Run the following command to generate a template for the package
```julia
using PkgTemplates
tpl = Template(; 
	user="my-username", 
	dir="~/code", 
	authors="Acme Corp", 
	julia=v"1.4", 
	plugins=[ 
		License(; name="MPL"), 
		Git(; manifest=true, ssh=true), 
		GitHubActions(; x86=true), 
		Codecov(), 
		Documenter{GitHubActions}(), 
		Develop(), 
		], )
tpl("PkgName")
```

2. Create an empty repository on Github.
![newpackage](github-newpackage.png)
Note that the default branch on Github may be set to "main". You need to go to "Setting/Repository Default" to change the default branch to be "master".

3. Run the commands to upload the local repository to Github.
```sh
git remote set-url origin https://github.com/numericalEFT/MCIntegration.jl.git
git branch -M master
git push -u origin master
```

4. Update the links for badges. Make sure that the links are pointed to the correct account hosting the repository.

5. Turn on support for documentation pages.
- Check docs/make.jl.  Make sure that the links are pointed to the correct account hosting the repository. The link in the following codes are particularly important for github to create the new branch "gh-pages" to host the documentations. The key "devbranch" in the following codes must be "master" instead of "main",
```julia
deploydocs(;
repo="github.com/numericalEFT/MCIntegration.jl",
devbranch="master",
)
```
- After pushing to github, make sure github generates the new branch "gh-pages". You may check the highlight parts in the following screenshot to check if the deployment to gh-pages branch is successful or not.
![deploy](github-doc-deploy.png)
- Go to "settings/pages", make the following settings for the documentation webpage
![doc](github-doc.png)
- It takes a couple of minutes for github to get the webpage ready.

6. When you are ready to register your package officially, add the following comment to the commit that is going to be released. You need to wait for ~3 days for the initial registration. You can use the same comment to create following-up releases (Don't forget to increase the version number in Project.toml.
```
@JuliaRegistrator register
```