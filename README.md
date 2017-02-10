# Chino's
My blog.
Powered by [Hexo](https://hexo.io/) .

Before add new post, modify _config.yml.


### After clone.

Install npm.
Install highlight.js submoudles.

``` bash
npm install

git submodule init
git submodule update
cd highlight.js/
node tools/build.js -t node
cd build
npm link
cd ../../
npm link highlight.js
```

### Update highlight.js

``` bash
git submodule update
cd highlight.js/
node tools/build.js -t node
```


### Add new post
``` bash
git fetch origin
git rebase origin/source
```

#### Code
``` bash
hexo new code "OJ::0001-NAME"
```
Mv to code

#### Diary
``` bash
hexo new diary "TITLE"
```
Mv to diary

#### note
``` bash
hexo new note "TITLE"
```
Mv to note


### Compile
``` bash
hexo g
```


### Testing
Start static http server.
``` bash
hexo server -s
```


### Deploy to github
``` bash
git add .
git commit -m 'XXX'
git push origin source
hexo g -d
```
