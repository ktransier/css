#### Sync local Postgres database with Digital Ocean droplet

```bash
#! /bin/bash
ssh root@107.107.107.107 'cd /home/deployer/quickdraw/current/ && pg_dump -Fc quickdraw_production > latest.dump'
scp root@107.107.107.107:/home/deployer/quickdraw/current/latest.dump ~/dropbox/code/retrograde
pg_restore --verbose --clean --no-acl --no-owner -j 2 -h localhost -d quickdraw_development latest.dump
ssh root@107.107.107.107 'cd /home/deployer/quickdraw/current/ && rm latest.dump'
rm latest.dump
```
