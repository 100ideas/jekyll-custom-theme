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
  - check `https://<your-github-username>.github.io/jekyll-custom-theme/` to see if the theme is working

4. clone to local machine (using new repo name in this case of 'jekyll-custom-theme'):

  ```
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

  ```
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

  ```
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

## Customizing the theme

Let's look at the differences between our local `master` branch and the `gh-pages` remote branch currently being published by github ('origin' is how we refer to the `remote` copy of our git repo hosted on github.com's servers; `upstream` refers to the original repo we forked). We can use the `--name-only` option to return a list of the changed files without the changes themeselves:

```
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

This listing shows us that the `gh-pages` branch on our remote has many different files when compared with our local `master` branch. Of particular note are the `.yml` files, which proide configuration options and structured metadata for jekyll. Let's start by editing the `_config.yml` file

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
