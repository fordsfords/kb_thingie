# kb_thingie
Experimental Knowledge Base repo.


## Table of contents

<!-- mdtoc-start -->
&bull; [kb_thingie](#kb_thingie)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [Table of contents](#table-of-contents)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [Introduction](#introduction)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [Quick Start](#quick-start)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [Add or Modify Content](#add-or-modify-content)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [Personalize](#personalize)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [More Details](#more-details)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [titleid](#titleid)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [The Process](#the-process)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [Problems](#problems)  
&nbsp;&nbsp;&nbsp;&nbsp;&bull; [License](#license)  
<!-- TOC created by './mdtoc.pl README.md' (see https://github.com/fordsfords/mdtoc) -->
<!-- mdtoc-end -->


## Introduction

I like Knowledge Bases (KB).

GitHub has a wiki feature that is pretty good, and can act as a KB
but GitHub prevents search engines (Google, etc.) from crawling repo wikis
except under certain circumstances
("[Note: Search engines will only index wikis with 500 or more stars that you configure to prevent public editing.](https://docs.github.com/en/communities/documenting-your-project-with-wikis/about-wikis)").

The only way I know to have GitHub-hosted content crawled by search engines
is to have the content show up on [GitHub Pages](https://pages.github.com/).
* Good news: easy to set up. Just create a branch named "`gh-pages`".
* Bad news: it only seems to work for HTML pages, not markdown pages.

So this KB let's you write in markdown and display in html.

BUT!!! This KB is difficult enough to use that I don't use it personally.
I.e. I just use the GitHub wiki, because I don't care if my personal wiki
is crawled by search engines.
See [Problems](#problems).


## Quick Start

This assumes that you have a GitHub account, a Linux machine, and have set up
your Linux "`git`" to be connected to your GitHub account.
In the examples below, your GitHub username is "coder503oops" and your
intended name for your new KB is "my_kb".

1. Create a new (empty) repo with your KB's name ("my_kb"). Do this using GitHub's web interface.

2. Clone it to your Linux machine. For example:
    ````
    git clone git@github.com:coder503oops/my_kb.git
    cd my_kb
    ````

3. Download into your (empty) KB repo a copy of the [kb_thingie](https://github.com/fordsfords/kb_thingie) repo.
For example:
    ````
    wget https://github.com/fordsfords/kb_thingie/archive/refs/heads/main.zip
    unzip main.zip
    mv kb_thingie-main/* .
    rm -r main.zip kb_thingie-main
    ````

4. Save this to GitHub. For example:
    ````
    git add .
    git commit -m "initial copy of my_kb"
    git push
    ````

5. Create the gh-pages branch and push it to GitHub. For example:
    ````
    # Only do these steps the first time to create it.
    git checkout -b gh-pages    # Only use "-b" the first time to create it.
    git push -u origin newbranch
    git checkout main
    ````

6. Wait a few minutes for your site to be deployed, then browse to it.
For example, if your GitHub username is "coder503oops", browse to
https://coder503oops.github.io/my_kb/html/home.html

## Add or Modify Content

Here's how I normally do it:
````
cd kb
cp sample-kb-page.md my-new-page.md
vi my-new-page.md    # Change the page title to "My New Page" and re-work the content.
vi home.md           # Add '[[My New Page]]' to other pages, as appropriate.
./bld.sh             # build it.
cd ../html
explorer.exe my-new-page.html  # I use WSL2, so this brings up the page in my browser.
cd ..

# Ready to commit
git add .
git commit -m "updates"
git push
# Deploy to web.
git checkout gh-pages
git merge main
git push
git checkout main

# Or, if you're feeling brave:
git add .;git commit -m "updates";git push;git checkout gh-pages;git merge main;git push;git checkout main
````

## Personalize

You might want to modify the "README.md" file.
It is not part of the KB itself.
For your KB, this file should not have much in it,
other than a link to the live KB site.

In "bld_1.sh" look for and change:
* `geeky-boy.com` and `whirlpool.jpg`
* `Steve Ford's Knowledge Base`
* Feel free to add other header and footer stuff. Look for `__HEADER__` and `__FOOTER__`.

In "kb/home.md" look for "fordsfords" and change appropriately.
And you'll probably want to change everything else in there too.

## More Details

The "source" files (under `kb`) use [GitHub Flavored Markdown](https://github.github.com/gfm/) with
a the following extensions:
1. [[Page Title]] - insert a KB link, using the title of the KB page.
The title is set by the top-level section header, usually the first line in the file.
For example:
    ````
    # Title for This Page
    ````
    See [titleid](#titleid) for more details.

    Note that unlike most KBs, this one does not support changing the link text.
    It has to be the page title.

2. Table of contents. For example:
    ````
    <!-- mdtoc-start -->
    <!-- mdtoc-end -->
    ````
    By inserting those lines, building the KB will modify the file to
    contain the markdown for an internal page table of contents.


## titleid

Section titles are written with leading hash symbols (`#`).
For example:
````
# Title for This Page

Abstract info.

## Introduction

Introductory Material.
````

You should only have one top-level section (with a single hash), and this servers as the page title.

All section titles (including the page title) go through a process to produce a title ID.
The full details of these steps are under development, but basically:
* Convert upper-case to lower-case
* Convert spaces to dashes
* Remove most (all?) special characters.

So, in the above example, the title ID for "`# Title for This Page`" is "`title-for-this-page`".
For the top-level page title, the title ID must match the name of the file, appended with ".md".
For lower-level sections, the title ID is used as an HTML anchor.
This all roughly matches what GitHub normally does anyway when it renders ".md" files.

## The Process

In the future, I would like to implement GitHub actions to build
the KB automatically on checkin of .md files.
For now, the build will have to be a manual step.

1. Clone the `kb_thingie` repo on your Linux host.
1. Update existing files and/or create new ones.
1. Run "./bld.sh"
1. Git add, commit, and push. Change branch to gh-pages. Git merge, push. Change branch back to main.
````
git add .; git commit -m 'updates'; git push; git checkout gh-pages; git merge; git push; git checkout main
````

## Problems

I do not expect this KB to get much use.
It is more of an experiment than a usable KB.
Right now, its only advantage is it can be crawled by search engines.

Here are its problems:
1. No web interface. You need to clone the repo, edit the files with your favorite text
editor, and build/push the results.
1. Linux only. The build tools are shell scripts.
1. No automatic page creation. You have to create the files yourself,
and even name them properly.
1. Slow to work on. An important feature of KB is the ease and low effort to
make simple changes. This KB requires a whole edit/build/push cycle.
1. No WYSIWYG. Most KB let you immediately see what your content will look like.
This KB has a form of preview (build/lanch browser), but it is not fast.

## License

1. This repo contains a copy of the Linux executable for the
[pmarkdown](https://metacpan.org/pod/App::pmarkdown) application,
version 1.06, which is used to build this KB.
It is copyright by Mathias Kende and
[MIT licensed](https://metacpan.org/dist/Markdown-Perl/view/script/pmarkdown#LICENCE).

2. It also shamelessly uses an old CSS style sheet from the [Doxygen](https://www.doxygen.nl/) project,
which is copyright by Dimitri van Heesch and [GNU Licensed](https://www.doxygen.nl/license.html).

3. Everything else is developed for the kb_thingie project, which is licensed as follows:

    I want there to be NO barriers to using this code, so I am releasing it to the public domain.  But "public domain" does not have an internationally agreed upon definition, so I use CC0:

    Copyright 2020 Steven Ford http://geeky-boy.com and licensed
    "public domain" style under
    [CC0](http://creativecommons.org/publicdomain/zero/1.0/):
    ![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png "CC0")

    To the extent possible under law, the contributors to this project have
    waived all copyright and related or neighboring rights to this work.
    In other words, you can use this code for any purpose without any
    restrictions.  This work is published from: United States.  The project home
    is https://github.com/fordsfords/kb_thingie

To contact me, Steve Ford, project owner, you can find my email address
at http://geeky-boy.com.  Can't see it?  Keep looking.
