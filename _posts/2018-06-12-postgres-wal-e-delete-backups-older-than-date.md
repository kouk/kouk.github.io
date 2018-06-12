---
published: false
title: 'Postgres/WAL-e: delete backups older than date'
tags:
- postgres
- wal-e
- backups
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
## Deleting backups older than a particular date with WAL-e

WAL-e has the `delete before` and the `delete retain` commands to help with cleaning up old backups. However it doesn't have an option to [delete backups older than a date](https://github.com/wal-e/wal-e/issues/206). The following script gets the job done:


<div class="line">{% gist kouk/3ba77edce12e95c1f779 shallow-submodule-add %}</div>
