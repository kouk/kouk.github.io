---
published: false
title: 'archive old revisions of a git repository'
tags:
- git
status: publish
type: post
published: true
meta:
  _wpas_skip_facebook: '1'
  _wpas_skip_google_plus: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_tumblr: '1'
  _wpas_skip_path: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  publicize_twitter_user: koukopoulos
  publicize_twitter_url: http://t.co/jfGqqYQ6aF
  _wpas_done_14391: '1'
  _publicize_done_external: a:1:{s:7:"twitter";a:1:{i:13858422;b:1;}}
author:
  login: kouk
  email: koukopoulos@gmail.com
  display_name: Konstantinos Koukopoulos
  first_name: ''
  last_name: ''
excerpt: !ruby/object:Hpricot::Doc
  options: {}
---
## Archive old revisions of a git repository

Sometimes one would like to continue development in a repository but without carrying older
historical revisions forward. To achieve this without actually rewriting the repository
history one can create a new detached timeline that can be reconnected later if necessary.

### Example

Let's imagine a repo like this:

    mkdir myrepo
    cd myrepo
    git init .
    echo 'this is new' > stuff.txt
    echo 'My repo' > README.txt
    git add -A
    git commit -m "initial revision"
    
After some time we have a long commit history in the `master` branch, and we have decided that we don't care anymore about `stuff.txt`:

    git rm -f stuff.txt
    git commit -m "remove old stuff"

However the `master` branch still contains revisions that contain that file.

To restart the timeline with a clean history we can create a new orphan branch with the exact same contents as our current `master`:

    head=$(git rev-parse master)
    git checkout --orphan newmaster master
    git commit -m "replaced with $head"

The `newmaster` branch doesn't contain any revisions with `stuff.txt` anymore! We can proceed to replace the `master` branch with the `newmaster` branch like this:

    git checkout -b oldmaster master  # save a local reference to the original history
    git checkout -B master newmaster
    git push --force

At this point, the local and remote `master` branch doesn't contain any of the commits
that exist in `oldmaster`. This is great because we don't have to carry forward references to the old files like `stuff.txt`. However it
is not that great if we still want to view the full commit history, e.g. for `README.txt`:

    git log master -- README.md

The above command will output only the commits made after our clean break. Additionally files that were removed before our clean break no longer appear in the history at all:

    git log master -- stuff.txt  # returns nothing

However if we have a reference to the old master branch
we can use `git replace` to create a synthetic history like this:

    start=$(git rev-list --max-parents=0 master)
    end=$(git rev-parse oldmaster)
    git replace $start $end
    git log master -- README.md
    git log master -- stuff.txt
    git replace -d $start

The output of `git log` will now be as if we had just continued development in the original branch!
