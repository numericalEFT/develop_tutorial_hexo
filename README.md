# Infrastructure

- The [website](https://numericaleft.github.io/developer_tutorial/) is powered by hexo 

- There are two relevant repos: [content repo](https://github.com/numericalEFT/develop_tutorial_hexo) and [webpage repo](https://github.com/numericalEFT/developer_tutorial)

- The theme is [minima](https://github.com/adisaktijrs/hexo-theme-minima) 

# How to use Hexo and deploy to GitHub Pages
* https://github.com/hexojs/hexo
* https://hexo.io/docs/
* https://gist.github.com/btfak/18938572f5df000ebe06fbd1872e4e39

## 1. Install Hexo
```
$ sudo npm install -g hexo-cli

$ hexo -v
hexo-cli: 0.1.9
os: Darwin 14.3.0 darwin x64
http_parser: 2.3
node: 0.12.7
v8: 3.28.71.19
uv: 1.6.1
zlib: 1.2.8
modules: 14
openssl: 1.0.1p
```

### 2. Add new post to the repo and deploy to Github
1. Clone the [content repo](https://github.com/numericalEFT/develop_tutorial_hexo) to local.
2. In the root folder of the local repo, run "npm install".
3. Run "hexo new post-name.md" to generate a new post in "source" folder. Note that the command will create a new markdown file as well as a folder for attachments.
4. Run "hexo g" to generate the website.
5. Run "hexo server" to preview the website on your local computer.
6. Run "hexo d" to deploy the new website to [webpage repo](https://github.com/numericalEFT/developer_tutorial)

