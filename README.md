# Chino's
My blog.
Powered by [Hexo](https://hexo.io/) .

Before add new post, modify _config.yml.


### After clone.
``` bash
npm install
```


### Add new Post
``` bash

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
hexo d
git add .
git commit -m 'XXX'
git push origin source
```
