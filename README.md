# Code for My Blog @ dwijaybane.github.io using Hugo with github pages

0. Install [Hugo](https://github.com/gohugoio/hugo/releases) and Go (conda setup), For Hugo download extented version.

1. Select some Hugo Theme you like, for example [gruvbox](https://themes.gohugo.io/themes/hugo-theme-gruvbox/)

2. Create a repo by name "blog" for simple purpose to save all your blogging code

3. Create a repo by your github username as its only allowed "dwijaybane.github.io"

4. Now clone blog repo `git clone https://github.com/dwijaybane/blog.git`

5. go to blog `cd blog`

6. Create new hugo site `hugo new site dwijaybaneblog`

7. Go to your site `cd dwijaybaneblog`

8. Create a new post
```bash
hugo new posts/hello-world.md
echo Hello World! >> content/posts/hello-world.md
```

9. Setting up theme
```bash
git submodule add https://github.com/schnerring/hugo-theme-gruvbox.git themes/gruvbox
```

10. Next, we will initialize hugo module. This piggybacks of go modules and will create a go.mod and go.sum.
```
hugo mod init dwijaybaneblog
```

11. Then, Open `vim config.toml`, First Change the default `baseURL, languageCode and title`. Then add a new line defining the theme.
```toml
baseURL = 'https://dwijaybane.github.io'
languageCode = 'en-us'
title = "Madamast's Blog"
theme = "gruvbox"
```

12. Add the following so the theme can resolve directories correctly.
```toml
[markup]
  # (Optional) To be able to use all Prism plugins, the theme enables unsafe
  # rendering by default
  #_merge = "deep"

[build]
  # The theme enables writeStats which is required for PurgeCSS
  _merge = "deep"

# This hopefully will be simpler in the future.
# See: https://github.com/schnerring/hugo-theme-gruvbox/issues/16
[module]
  [[module.imports]]
    path = "github.com/schnerring/hugo-mod-github-readme-stats"
  [[module.imports]]
    path = "github.com/schnerring/hugo-mod-json-resume"
    [[module.imports.mounts]]
      source = "data"
      target = "data"
    [[module.imports.mounts]]
      source = "layouts"
      target = "layouts"
    [[module.imports.mounts]]
      source = "assets/css/json-resume.css"
      target = "assets/css/critical/44-json-resume.css"
  [[module.mounts]]
    # required by hugo-mod-json-resume
    source = "node_modules/simple-icons/icons"
    target = "assets/simple-icons"
  [[module.mounts]]
    source = "assets"
    target = "assets"
  [[module.mounts]]
    source = "layouts"
    target = "layouts"
  [[module.mounts]]
    source = "static"
    target = "static"
  [[module.mounts]]
    source = "node_modules/prismjs"
    target = "assets/prismjs"
  [[module.mounts]]
    source = "node_modules/prism-themes/themes"
    target = "assets/prism-themes"
  [[module.mounts]]
    source = "node_modules/typeface-fira-code/files"
    target = "static/fonts"
  [[module.mounts]]
    source = "node_modules/typeface-roboto-slab/files"
    target = "static/fonts"
  [[module.mounts]]
    source = "node_modules/@tabler/icons/icons"
    target = "assets/tabler-icons"
```

13. Run the following commands to initialize the theme.
```bash
hugo mod get
hugo mod npm pack
npm install
```

14. JSON Resume, This theme uses [schnerring/hugo-mod-json-resume](https://github.com/schnerring/hugo-mod-json-resume)
for a bunch of things, including populating sidebar.
To personalize create a new file `mkdir -p data/json_resume`, `vim data/json_resume/en.json`
```json
{
  "$schema": "https://raw.githubusercontent.com/jsonresume/resume-schema/v1.0.0/schema.json",
  "basics": {
    "name": "Dwijay Bane",
    "image": "https://avatars.githubusercontent.com/u/2122671?s=400&u=85207688a39bb18c5584b65b32ee13f4185f26ff&v=4",
    "email": "dwijaybane2@gmail.com",
    "url": "https://dwijaybane.github.io",
    "profiles": [
      {
        "network": "Github",
        "username": "dwijaybane",
        "url": "https://github.com/dwijaybane"
      }
    ]
  }
}
```

15. Now go out of the root and clone your main blog repo
```
git clone https://github.com/dwijaybane/dwijaybane.github.io.git
git checkout -b main
touch README.md
git add .
git commit -m "adding readme"
git push origin main
```

16. Go back to site repo
```
cd blog/dwijaybaneblog
hugo server
```

17. If you get problem as below:
```
hugo: downloading modules …
hugo: collected modules in 2241 ms
Start building sites …
hugo v0.111.3-5d4eb5154e1fed125ca8e9b5a0315c4180dab192+extended linux/amd64 BuildDate=2023-03-12T11:40:50Z VendorInfo=gohugoio
Error: Error building site: JSBUILD: failed to transform "js/flexsearch.js" (text/javascript): "/home/dwijay/Documents/blog/dwijaybaneblog/themes/gruvbox/assets/js/flexsearch.js:75:2": Unexpected ";"
```

- To solve `vim themes/gruvbox/assets/js/flexsearch.js` go to 73 line.
```
   index.add(
     {{ range $index, $element := $list }}
       {
         id: {{ $index }},
         href: "{{ .RelPermalink }}",
         title: {{ .Title | jsonify }},
         {{ with .Description }}
           description: {{ . | jsonify }},
         {{ else }}
           description: {{ .Summary | plainify | jsonify }},
         {{ end }}
         content: {{ .Plain | jsonify }}
       }
       {{ if ne (add $index 1) $len }}
         .add(
       {{ end }}
     {{ end }})
```

18. If you don't see your first post
```bash
vim content/posts/hello-world.md
remove draft line and save
hugo server
```

19. Now to Generate Static Pages and Deploy
```bash
rm -rf public
git submodule add -b main https://github.com/dwijaybane/dwijaybane.github.io.git public
cd public
git remote -v
cd ..
hugo -t gruvbox
cd public && ls
git status
git add .
git commit -m "init commit"
git push origin main
```

20. Now go to your site github repo `dwijaybane.github.io`. Go to Settings -> Pages

21. Select Branch -> main, it should show show `Your site is live at ...`

## References
1. [How to Setup gruvbox with hugo](https://blog.kgb33.dev/posts/getting_started_with_hugo/)
2. [Youtube video for deploying it to github.io](https://www.youtube.com/watch?v=LIFvgrRxdt4)



