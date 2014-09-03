
This composite plugin (rake task + plugin) allows you to import a Github wiki in a Jekyll site.

## install

### Create your wiki

**Note** : your wiki MUST be public and have at least One page

### Download code

- Download the [zip archive here](https://github.com/djacquel/WikiToJekyll/archive/master.zip)
- Unzip in an already existing Jekyll folder

### Link your Wiki to your repository as a submodule

```
git submodule add https://github.com/userName/repositoryName.wiki.git
git submodule init
git submodule update
```

### Configure

Go to your **_config.yml** file and set you parameters for wiki import


```
##
# Parameters for WikiToJekyll
#
# Used by Rake::wiki and wikiLinks generator plugin
##

wikiToJekyll:
  # set your remote name. 'origin' is the default name set
  # when you do a 'git init'
  # if you changed this name be sure to change this parameter
  deploy_remote: "origin"

  # for an organization / user, publication branch is master
  # for a project, publication branch is gh-pages
  deploy_branch: "gh-pages"

  # code repository url
  # IMPORTANT: no git@github.com: in front
  #            You MUST use the htpps:// url or deriving wiki url
  #            will not generate a proper url for git submodule
  #            and then cause an error on github
  #            https://github.com/userName/repositoryName
  repository_url: "https://github.com/userName/repositoryName"

  # wiki url
  # if you live this blank, it will be derived from you code
  # repository url
  #   eg : wiki_repository_url = repository_url + '/wiki'
  #
  # If you're importing a wiki from another code repository
  # you MUST set this url
  #
  # IMPORTANT: no git@github.com: in front
  #            You MUST use the htpps:// url or it will
  #            cause a submodule error on github
  #            https://github.com/userName/repositoryName/wiki
  wiki_repository_url:

  # wiki submodule folder
  wiki_source: "_wiki"

  # wiki Jekyll generated pages destination folder
  wiki_dest: "wiki"

  # commit and push to Jekyll repository after wiki synchronisation
  commit_and_push: false
```

If your site is a repository site (ie: living under user.github.io/repositoryName), **don't forget** to set your **baseurl:** parameter in your **_config.yml** file.

```
baseurl: '/repositoryName' # no trailing slash !
```

### Import your wiki

Synchronizing your wiki can be done with the associated rake task :

```
rake wiki
```

This will :

- cause errors if you've missed some of the install steps
or
- download you wiki pages
- transform them to markdown pages with yaml front matter
- launch a Jekyll build
- convert wiki links to Jekyll links
- optionally commit and push you code to you Jekyll repository

This has been made as a rake task, because some may want to make a cron job with this.


## Jekyll pages integration

By default, all wiki pages are in the **wiki** folder and all have a **menu: wiki** variable set in the front matter.

A wiki menu can be easily generated with this markup :

```
<ul class="wikiMenu">
  {% for p in site.pages %}
    {% if p.menu == "wiki" %}
    <li><a class="post-link" href="{{ p.url | prepend: site.baseurl }}">{{ p.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>
```

## Todo

The wiki mardown is a little different from the default Jekyll markdown.

```
... end of sentence
## title
- list item
- list item
```
That renders in wiki but not in Jekyll.
