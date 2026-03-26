---
author: xbr
title: Android Security Resources
//subtitle: collection of resources I found useful
date: 2024-06-28 
Lastmod: 2024-06-28
description: collection of resources I found useful 
categories:
  - technical
tags:
  - android
  - resources

---
---
Resources/blogs that I found uselful (this list will be updated often)
<!--more-->
[android-component-security](https://www.hebunilhanli.com/wonderland/mobile-security/android-component-security/)

[NVIDO Labs articles](https://blog.nviso.eu/category/mobile-security/)

[Oversecured articles](https://blog.oversecured.com/)

[Android bascis ppts](https://valsamaras.medium.com/android-security-workshop-5eadeb50fba)

[canyie blogs](https://blog.canyie.top/)

[wrlus blogs](https://wrlus.com/)

[Evolution of Android Security]( https://github.com/balazsgerlei/AndroidSecurityEvolution)

# Mobile Disclosed Vulnerability Reports

---

## Webview 

### Webivew Takeover
 - https://securitylab.github.com/advisories/GHSL-2023-142_Home_Assistant_Companion_for_Android/

### JS Execution 
 - https://hackerone.com/reports/499348

### HSTS Webview ATO
 - https://seanpesce.blogspot.com/2024/09/exploiting-android-client-webviews-with.html --> https://httpsredirector.com/#u=https://example.com

### JS Bridge
 - https://medium.com/@FufuFaf1/uxss-in-opera-gx-android-from-error-page-to-native-bridge-escalation-69eb0baae576
 - https://hackerone.com/reports/1343300
 - https://medium.com/@youssefhussein212103168/exploiting-insecure-android-webview-with-javascript-interface-a4d3abf9ec09
 - https://inesmartins.github.io/exploiting-deep-links-in-android-part-2/index.html?ref=localhost
 - https://hulkvision.github.io/blog/javascript-interface/exploiting-javascript-interface/

### UXSS
 - https://hackerone.com/reports/906433

### Webview LFI
 - https://medium.com/@srilakivarma/exploiting-insecure-webviews-xss-and-lfi-fe5109719bd5

### Stored XSS
 - https://medium.com/@srilakivarma/how-i-found-a-critical-stored-xss-in-a-financial-app-ios-android-eac1447e9702

### WebResourceResponse
- https://blog.oversecured.com/Android-Exploring-vulnerabilities-in-WebResourceResponse/

### Webview Cookie Cache Poisoning
 - https://issues.chromium.org/issues/40284849
 - https://blog.oversecured.com/Android-security-checklist-webview/#installing-cookies-for-third-party-sites

### Open Redirect
 - https://nvd.nist.gov/vuln/detail/CVE-2023-49104
 - https://hackerone.com/reports/499348
 - https://hackerone.com/reports/2555949
 - https://hackerone.com/reports/1343300
 - https://bughunters.google.com/reports/vrp/9RRUiSU3k/report

## Android backup enabled
 - https://hackerone.com/reports/12617

## Content Provider
 - https://securitylab.github.com/advisories/GHSL-2021-1007-Nextcloud_Android_app/
 - https://securitylab.github.com/advisories/GHSL-2022-059_GHSL-2022-060_Owncloud_Android_app/
 - https://blog.ostorlab.co/signal-arbitrary-file-read.html
 - https://www.rapid7.com/blog/post/cve-2025-10184-oneplus-oxygenos-telephony-provider-permission-bypass-not-fixed/

## SDK
 - https://poneglyph.cloud/how-a-misconfigured-android-sdk-can-lead-to-stealthy-camera-abuse/

## Firebase Misconfig 
 - https://cyberweapons.medium.com/misconfigured-firebase-db-on-both-android-and-web-apps-a85927e4678f
 - https://medium.com/@pentestersuresh01/how-i-found-a-critical-account-takeover-via-firebase-api-misconfiguration-fa9ddd2ef114
 - https://appsecwriteups.com/how-to-identify-and-handle-firebase-security-misconfigurations-a-case-study/
 - https://medium.com/@mustafamohammed789mm/firebase-misconfigurations-from-discovery-to-exploitation-0a282b81ad4f

## Permission bypass
 - https://www.youtube.com/watch?v=pP5tKT9-I0Y

## Path traversal
 - https://hackerone.com/reports/1377748

## SetResult Permission Abuse
 - https://securitylab.github.com/advisories/GHSL-2021-1033_Nextcloud_News_for_Android/

## Broadcast Receiver
 - https://hackerone.com/reports/289000?ref=infosecmania.com

## Custom Tabs
 - https://github.com/advisories/GHSA-x9h6-qwxm-528g

## Custom Scheme Hijacking
 - https://medium.com/@AlQa3Qa3_M0X0101/how-i-was-able-to-get-two-account-takeovers-via-oauth-custom-scheme-hijacking-at-the-same-target-6a6980ebbac1
 - https://hackerone.com/reports/855618?ref=localhost
 - https://inesmartins.github.io/exploiting-deep-links-in-android-part1/index.html
 - https://security.lauritz-holtmann.de/post/sso-android-autoverify/#scenario-1-sso-flow-no-legitimate-app

## RCE
 - https://hackerone.com/reports/1115864
 - https://hulkvision.github.io/blog/post1/

## OAuth ATO
 - https://github.com/google/security-research/blob/80f0262a02cc74fc5e28b4786edf210aba2e9954/advisories/GHSA-6r3h-49f8-wwph.md?plain=1#L70
 - https://blog.ostorlab.co/one-scheme-to-rule-them-all.html

## ABSOLUTE CINEMA
 - https://bugscale.ch/blog/shoot-for-the-galaxies-our-samsung-s25-1-click-rce-journey/
 - https://www.youtube.com/watch?v=B0A8F_Izmj0
 - https://www.nccgroup.com/media/vodcuxpw/samsung-galaxy-s24-whitepaper.pdf

## Host Validation Techniques
 - https://www.hahwul.com/blog/2019/bypass-host-validation-technique-in-android/

## BROWSABLE Bypass
 - https://bughunters.google.com/reports/vrp/Tkj9d5vwo

## AI for Mobile
 - https://github.com/trailofbits/skills/tree/main/plugins/firebase-apk-scanner