---
title: "{{ replace .Name "-" " " | title }}"
date: {{ now.Format .Site.Params.dateFmt }}
draft: false
categories:
  - uncategorized
---
