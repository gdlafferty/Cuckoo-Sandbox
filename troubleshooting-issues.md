# Troubleshooting/Issues Encountered During Install

## Errno 24 when running cuckoo -d
Received [IOError: [Errno 24] Too many open files](https://cuckoo.sh/docs/faq/index.html#ioerror-errno-24-too-many-open-files) when running ```cuckoo -d```, followed instructions outlined in [this post](https://easyengine.io/tutorials/linux/increase-open-files-limit/) and rebooted.

The issue persisted, which was verified by checking /etc/security/limits.conf and seeing the correct values set from the blogpost. Running ```ulimit -n``` returned a value of 1024. Running ```su cuckboi``` then repeating the ```ulimit -n``` produced a value of 500000 as expected.

Modified the limit used by the login shell @ /etc/systemd/user.conf by adding the line ```DefaultLimitNOFILE=500000``` to the end of the file, which affects the soft limit but doesn't address the hard limit. Adding the same line to the end of /etc/systemd/system.conf will affect the hard limit.

Rebooting again and running ```ulimit -n``` fixed the issue.

## jsondump:encoding not found error when running cuckoo -d
Received ```[cuckoo.common.config] ERROR: Type of config parameter reporting:jsondump:encoding not found!``` when running ```cuckoo -d```. Editing $CWD/conf/reporting.conf and removing the line ```encoding = utf-8``` resolved the issue.

