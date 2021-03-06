# Setting up a github-pages repo with a custom jekyll theme

1. find a theme you like
  - free themes: http://jekyllthemes.org
    - https://github.com/thomasvaeth/trophy-jekyll
    - https://github.com/ninapetrop/Artist-Theme
    - https://mmistakes.github.io/skinny-bones-jekyll/getting-started/  ([source](https://github.com/mmistakes/skinny-bones-jekyll))
  - premium themes: http://www.jekyllthemes.io

2. Let's use [skinny-bones-jekyll](https://github.com/mmistakes/skinny-bones-jekyll). Fork it on github by clicking the the 'Fork' button.

3. rename your new (forked) repo of the theme. settings > rename:
  - rename as desired
  - note branch name listed under **github pages** ("master" or "gh-pages") - gh-pages in this case
  - check `https://<your-github-username>.github.io/jekyll-custom-theme/` to see if the theme is working - in my case, https://100ideas.github.io/jekyll-custom-theme/

4. clone to local machine (using new repo name in this case of 'jekyll-custom-theme'):

  ```bash
  $ git clone git@github.com:100ideas/jekyll-custom-theme.git
  Cloning into 'jekyll-custom-theme'...
  Resolving deltas: 100% (613/613), done.

  $ cd jekyll-custom-theme
  $ git branches
  * master
    remotes/origin/HEAD -> origin/master
    remotes/origin/develop
    remotes/origin/gh-pages
    remotes/origin/master
  ```

5. add the original repository as an upstream remote so we can pull future updates:

  ```bash
  $ git remotes
  origin	git@github.com:100ideas/jekyll-custom-theme.git (fetch)
  origin	git@github.com:100ideas/jekyll-custom-theme.git (push)

  $ git remote add upstream https://github.com/mmistakes/skinny-bones-jekyll.git
  $ git remotes
  origin	git@github.com:100ideas/jekyll-custom-theme.git (fetch)
  origin	git@github.com:100ideas/jekyll-custom-theme.git (push)
  upstream	https://github.com/mmistakes/skinny-bones-jekyll.git (fetch)
  upstream	https://github.com/mmistakes/skinny-bones-jekyll.git (push)
  ```

6. Fetch the branches and their respective commits from the upstream repository. Commits to `master` will be stored in a local branch, `upstream/master`.

  ```bash
  $ git fetch upstream
  From https://github.com/mmistakes/skinny-bones-jekyll
   * [new branch]      develop    -> upstream/develop
   * [new branch]      gh-pages   -> upstream/gh-pages
   * [new branch]      master     -> upstream/master

  $ git remotes
  origin	git@github.com:100ideas/jekyll-custom-theme.git (fetch)
  origin	git@github.com:100ideas/jekyll-custom-theme.git (push)
  upstream	https://github.com/mmistakes/skinny-bones-jekyll.git (fetch)
  upstream	https://github.com/mmistakes/skinny-bones-jekyll.git (push)

  # `*` tells us what branch HEAD points at, in this case `master`
  $ git branches
  * master
    remotes/origin/HEAD -> origin/master
    remotes/origin/develop
    remotes/origin/gh-pages
    remotes/origin/master
    remotes/upstream/develop
    remotes/upstream/gh-pages
    remotes/upstream/master
  ```

7. To get updates from now on, `git fetch upstream` will pull updates to the theme and store them in `upstream`. Use `get merge upstream/master` to merge changes with the current branch (more info below).

## removing theme demo content (quick git tutorial)

**On forks**: When we forked the theme on github.com, our new repo was initialized with copies of all the branches in the source repo (`develop`, `master`, & `gh-pages` as above). The `gh-pages` branch still has the content for the demo site from the source repo, while the `master` branch contains the bare minumum needed to get started with the theme.

**On remotes**: "Remotes" are separate repos that git tracks for synchronizing (`push`ing or `pull`ing) with the local repository. The remote version of a repository hosted by github is typically named `origin`, and `remote/origin/master` refers to the `master` branch in the copy of the repo on github's servers; `remote/upstream/master` refers to the `master` branch in the repo that was the source of the fork after we've added it as above).

Let's use `git diff` to explore the differences between our local `master` branch and the `gh-pages` remote branch currently being published by github. We can use the `--name-only` option to return a list of the changed files without the changes themeselves:

```bash
$ git fetch upstream
$ git diff --name-only master upstream/gh-pages
README.md
_config.yml
_data/authors.yml
_data/footer.yml
_data/messages.yml
_data/navigation.yml
_includes/footer.html
_layouts/home.html
_posts/articles/2011-03-10-sample-post.md
_posts/articles/2012-05-22-readability-post.md
  # ... etc
index.md
media/index.md
terms/index.md
```

This listing shows us that the `gh-pages` branch on our remote has many different files when compared with our local `master` branch. Of particular note are the `.yml` files, which proide configuration options and structured metadata for jekyll. We can look at the differences of all the `.yml` files with `git diff master upstream/gh-pages */*.yml`.

1. Let's start by editing the `_config.yml` file. According to the [theme documentation](https://mmistakes.github.io/skinny-bones-jekyll/getting-started/#site-setup), the `url` parameter

  > is used to generate absolute URLs in `sitemap.xml`, `atom.xml`, and for generating canonical URLs in `<head>`. When developing locally either comment this out or use something like `http://localhost:4000` so all assets load properly. *Don't include a trailing `/`*.

  switch to your master branch (`git checkout master`) and add your site's absolute github.io url to `_config.yml` like so:

  ```
  url: http://100ideas.github.io/jekyll-custom-theme
  ```

2. commit your changes to master, merge them into a new local `gh-pages` branch, and push it to github, --force to replace remote branch ([animated git review](http://learngitbranching.js.org/?NODEMO&command=level%20intro3)):

  ```bash
  $ git status
  On branch master
  Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   _config.yml
  $ git add .
  $ git commit -m "add github.io absolute url"
  [master 3470a66] add github.io absolute url
   1 file changed, 1 insertion(+), 1 deletion(-)  

  # create new branch (based on HEAD) and switch to it
  $ git checkout -b gh-pages
  Switched to a new branch 'gh-pages'

  # no difference - new branch starts w/ contents of HEAD (* gh-pages)
  $ git diff master

  # what's the difference between the local gh-pages branch and remote master branch?
  # answer: line 5 of _config.yml
  $ git diff origin/master
  diff --git a/_config.yml b/_config.yml
  index 819ea61..1746c42 100644
  --- a/_config.yml
  +++ b/_config.yml
  @@ -5,7 +5,7 @@ description: "This is a description of my awesome site."
   logo: # 120x120 px default image used for Twitter summary card
   teaser: # 400x250 px default teaser image used in image archive grid
   locale: en
  -url:
  +url: https://100ideas.github.io/jekyll-custom-theme
   feed:
     path: atom.xml

  # origin gh-pages branch already exists & is full of demo content. Instead of
  # merging local changes into the remote branch (normal behavior of git push),
  # use git push --force to completely replace remote branch with local content
  $ git push --force origin gh-pages
  ```
  http://<your-github-username>.github.io/jekyll-custom-theme should now be mostly blank. If it looks like the css styling or JS is broken, check the web inspector console for errors and the url specified in `_config.yml`.

## Using jekyll

0. jekyll folder conventions - what gets served?
1. replace default images; commit;
2. `git ls-tree upstream/gh-pages about` look for about folder in history
3. `git checkout upstream/gh-pages -- articles`; commit; merge into gh-pages;

### workflow
1. plan:
  - option 1: use master (or other branches) for local development; install ruby, gem, bundler, jekyll; use jekyll to preview locally; commit often; merge changes into gh-pages for "production"; push to origin
  - option 2: commit changes to master or gh-pages & push directly to origin; mostly use github.com online txt editor to author content

git push --force

# About the theme we're using - Skinny Bones Jekyll Starter

To learn more about how to use the theme and install it check out the [Skinny Bones demo](http://mmistakes.github.io/skinny-bones-jekyll/) (*work in progress*).

![screenshot of Skinny Bones](http://mmistakes.github.io/skinny-bones-jekyll/images/skinny-bones-theme-feature.jpg)

---

## Notable Features

* Jekyll 3.x and GitHub Pages compatible.
* Stylesheet built using Sass.
* Data files for easier customization of the site navigation/footer and for supporting multiple authors.
* Optional Disqus comments, table of contents, social sharing links, and Google AdSense ads.
* And more.
