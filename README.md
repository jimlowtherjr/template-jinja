# template-jinja

# Example of getting the local facts
```
[root@boladm02 jlowther]# ansible -m setup -i jlowtherwnac1, all | sed "1c {" | jq ".ansible_facts.ansible_local.purpose"
{
  "bill_to": "Operations - Servers",
  "build_date": "11/02/2018",
  "classification": "DEV",
  "contact_group": "mti_unix_l2",
  "managed_by": "ECP Unix",
  "requestor": "jlowther",
  "sox_in_scope": "no",
  "support_tier": "4",
  "used_by": "jlowther"
}
```
# Variables, and example values
```
purpose_bill_to:        "Operations - Servers"
purpose_requestor:      "jlowther"
purpose_build_date:     "11/02/2018"
purpose_used_by:        "jlowther"
purpose_contact_group:  "mti_unix_l2"
purpose_classification: "DEV"
purpose_sox_in_scope:   "no"
purpose_managed_by:     "ECP Unix"
purpose_support_tier:   "4"
```

# System build date can be found by using tune2fs on the boot device
```
date --date="$(tune2fs -l /dev/sda1 | grep 'Filesystem created:' | sed -e "s/  / /g;s/  / /g;s/  / /g" | cut -d ' ' -f3-)" "+%Y-%m-%d"

[root@jlowther-lnx root]# date --date="$(tune2fs -l $(df -hlP | egrep /boot$ | cut -d ' ' -f 1)| grep 'Filesystem created:' | sed -e "s/  / /g;s/  / /g;s/  / /g" | cut -d ' ' -f3-)" "+%Y-%m-%d"
2020-01-03
```

# System build date can be inferred from the creation date on /root/anaconda-ks.cfg
```
[root@jlowther-lnx root]# stat /root/anaconda-ks.cfg | grep Change | cut -d ' ' -f 2
2020-01-03
```
