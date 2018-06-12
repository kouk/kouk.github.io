---
published: false
title: 'Postgres/WAL-e: delete backups older than date'
---
## Deleting backups older than a particular date with WAL-e

WAL-e has the `delete before` and the `delete retain` commands to help with cleaning up old backups. However it doesn't have an option to [delete backups older than a date](https://github.com/wal-e/wal-e/issues/206). The following script


<div class="line">{% gist kouk/3ba77edce12e95c1f779 shallow-submodule-add %}</div>
