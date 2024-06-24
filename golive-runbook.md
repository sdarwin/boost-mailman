
## Steps to deploy mailing lists

Many of the systems engineering notes ("launch server instances in the cloud", etc.) have not yet be included in this file.  

For now, let's focus on certain list details.   

## Create/Import lists

Note: https://lists.boost.org.cpp.al hosts archived html versions of the lists. This could act as a historical record,   
and remove the need to import lists which are otherwise no longer active.  

| List | Description | Recent Message | Active | Subject Prefix |
| ---- | ----------- | -------------- | ------ | -------------- |
| Boost | Boost developers' mailing list | 2024-06-24 | Yes | [boost]_ |
| Boost-announce | Boost announce-only mailing list | 2024-04-15 | Yes | [Boost-announce]_ |
| Boost-bugs |	Bugs reported/updated through Boost's Trac | 2023-03-21 | No |
| Boost-build |	Boost.Build developer's and user's list | 2024-02-08 | No |
| Boost-cmake |	Discussion of the CMake-based build system for Boost | 2012-11-19 | No |
| Boost-commit |	Boost Subversion commit messages | 2013-11-23 | No |
| Boost-docs |	Discussion of Boost Documentation | 2022-07-01 | No | 
| Boost-gil |	[no description available] | 2024-01-10 | Questionable | [Boost-gil]_ |
| Boost-Interest |	Moderated announcements of interest to the Boost community. | 2022-08-01 | No |
| Boost-maint |	Boost community maintenance mailing list | 2020-06-02 | No |
| Boost-mpi |	Discussion of Boost.MPI development | 2023-02-19 | No |
| Boost-Testing |	Running Boost regression tests | 2023-07-27 | No |
| Boost-users |	Boost Users mailing list | 2024-05-29 | Yes | [Boost-users]_ |
| Boost-www |	Discussion of boost.org website development. | 2022-04-16 | No |
| Geometry |	Boost.Geometry library mailing list | 2023-03-27 | No |
| glas |	Generic Linear Algebra Software | 2010-05-18 | No |
| Osl-test2 |	[no description available] | unknown | No |
| proto |	Discussions about Boost.Proto and DSEL design | 2016-04-14 | No |
| test |	[no description available] | 2015-06-02 | No |
| Test-ciere |	Testing email list | unknown | No |
| threads-devel |	Discussions about the boost.thread library | 2016-06-30 | No |
| ublas |	ublas mailing list | 2021-04-10 | No |

Contact Mateusz Loskot and ask about Boost-gil list.  

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

