---
author: xbr
title: Pearl CTF writeup
//subtitle: nah
date: 2025-03-08 
Lastmod: 2024-03-08
//description:  
categories:
  - technical
tags:
  - CTF
  - Writeup
  - Android

---
challenge name: ShadowVault

decription: A mysterious app called ShadowVault has surfaced, rumored to hide secrets within its code. Can you unravel its mystery?

category: rev

attachment: app-debug.apk

__TLDR;__

After logging into the app using hardcorded credentials, changing the values in request body which is also hardcoded gives the flag.

First up seeing the "rumored to hide secrets within its code" in description I started searching for any possible hardcoded values and discovered the login creds

![Credentials](/pearlCTF2025/pearl1.png)

Got the Credentials so its time to go dynamic analysis. Installing the apk in the android device showed the screen for credentials. After login in the next activity there was a button with name flag on it. 

![After Login](/pearlCTF2025/pearl2.png)

After pressing the button and capturing the traffic through HTTPToolkit got the request was going to the https://pearlctf[.]pythonanywhere[.]com/location. There where longitude and latitude as the request body. 

![Request Body](/pearlCTF2025/pearl3.png)

In the MainActivity we can see the values for longitude and latitude, again hardcoding! 

![Longitude and latitude](/pearlCTF2025/pearl4.png)

Changing the longitude and latitude values in the request to 200 and 100 in the request gives the flag in the response.

flag : pearl{r3v3rs3_c4ptur3_3xpl0it}