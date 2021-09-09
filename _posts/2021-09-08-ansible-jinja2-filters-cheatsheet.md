---
title: Ansible/Jinja2 filters Cheatsheet
date: 2021-09-08 19:00:00 +0000
categories: [Jinja2, Cheatsheets]
tags: [cheatsheets, jinja2, ansible, filters]
---
Nuestro objeto:
{% raw  %}
```json
"hostvars[inventory_hostname].ansible_devices": {
    "dm-0": {
        "holders": [],
        "host": "",
        "links": {
            "ids": [
                "dm-name-ubuntu--vg-root--lv",
                "dm-uuid-LVM-jSQ2nVM2g70g9zwebXlPMsVkrlarjYIIDFYDTlLgt8EWbixCWpBQlWRqAYg9II0R"
            ],
            "labels": [],
            "masters": [],
            "uuids": [
                "21f9a77a-4bb6-477a-842b-b5836297c95d"
            ]
        },
        "partitions": {},
        ...
    },
    "dm-1": { ... },
    "loop0": {
        "holders": [],
        "host": "",
        "links": {
            "ids": [],
            "labels": [],
            "masters": [],
            "uuids": []
        },
        "partitions": {},
        ...
    },
    "loop1": { ... },
    "loop2": { ... },
    "loop3": { ... },
    "sda": {
        "holders": [],
        ...
    }
}
```
{% endraw  %}

## Map attribute and combine
{% raw  %}
```yaml
partitions: "{{ hostvars[inventory_hostname].ansible_devices.values() | map(attribute='partitions') | reject('==',{}) | list | combine }}"
```
{% endraw  %}

{% raw  %}
```json
"partitions": {
  "sda1": { "holders": [], ...  },
  "sda2": { "holders": [], ...  },
  "sda3": {
    "holders": [
      "ubuntu--vg-var_log",
      "ubuntu--vg-root--lv"
    ],
    ...
  },
  "sda4": { ... }
}
```
{% endraw  %}
