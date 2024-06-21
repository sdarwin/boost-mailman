
## Steps to deploy mailing lists

Many of the systems engineering notes ("launch server instances in the cloud", etc.) have not yet be included in this file.  

For now, let's focus on certain list details.   

## Create/Import lists

Boost: Boost developers' mailing list  
Boost-announce: Boost announce-only mailing list  
Boost-users: Boost Users mailing list  

## Configure Lists

Recommended changes. After creating a list in the UI, make the following updates to the list:  
Settings -> List Identity -> Subject prefix -> [boost] (lower case to match previous list, and fix 'new subject' line)  
Settings -> DMARC mitigations -> DMARC mitigation action -> Replace From with list address. Save changes.  
Settings -> Alter messages -> Reply goes to list -> Reply goes to list  
Settings -> Alter messages -> Filter Content -> Yes  
Settings -> Alter messages -> Convert html to plaintext -> Yes. Save changes.  

## Import archives

on mailman server:  

```
#!/bin/bash

set -xe

servername=$(hostname -f)

. /opt/mailman3/bin/activate

cd /var/lib/mailman3/web/project
export PYTHONPATH=$PYTHONPATH:$PWD
export DJANGO_SETTINGS_MODULE=settings

mailman-web hyperkitty_import --since 1970 -l boost-announce@${servername} /opt/wowbagger/home/mailman-archives/private/boost-announce.mbox/boost-announce.mbox
mailman-web update_index_one_list boost-announce@${servername}

mailman-web hyperkitty_import --since 1970 -l boost-users@${servername} /opt/wowbagger/home/mailman-archives/private/boost-users.mbox/boost-users.mbox
mailman-web update_index_one_list boost-users@${servername}

mailman-web hyperkitty_import --since 1970 -l boost@${servername} /opt/wowbagger/home/mailman-archives/private/boost.mbox/boost.mbox.total
mailman-web update_index_one_list boost@${servername}
```

