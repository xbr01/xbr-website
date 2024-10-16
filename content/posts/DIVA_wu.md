---
author: xbr
title: "Android APK Analysis: Part One - DIVA"
//subtitle: nah
date: 2023-09-13
Lastmod: 2023-09-13
//description:  
categories:
  - technical
tags:
  - android
  - apk
  - vulnerability
---

This is the first intentional vulnerable APK I'm analysing. Without further ado, let's get into it

<!--more-->

---

1.**Insecure logging**

In the hint of the challenge it was given that insecure logging occurs when developers intentionally or unintentionally log the sensitive info that the user have entered such as credentials,session IDs etc.

First of all I opened the apk using *jadx.* Then I opened the package and found many activity classes. I proceeded to the *LogActivity* class as it is the activity class for this challenge. And yes, inside the *checkout* method I found that the text that we enter is been logged.

![Untitled](/diva_APK/1.png)

So to try it out I just entered some input on the app and to viewed the log.

To view the logs of an app you can use the command:

`adb shell logcat` 

![Untitled](/diva_APK/2.png)

![Untitled](/diva_APK/3.png)

So yeah it is indeed being logged!

---

2.**Hardcoding Issues - Part 1**

So first of all I searched for what hardcoding is and found out that it is leaving the sensitive information in the source code without encrypting. Know more [here](https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_password)

![Untitled](/diva_APK/4.png)

Then I analysed the APK using *jadx* looking for hardcoded data. In the *HardcodeActivity* we can see a *access* method in which there is a if-else condition which checks if the input in the edit text box in the app is equal to a string(vendorsecretkey) mentioned there and grants permission if it satisfies, and yeah we have found the hardcoded string.

![Untitled](/diva_APK/5.png)

---


3.**Insecure data storage - Part 1**
In *InsecureDataStorage1Activity* we can see that the username and password is stored in the shared preference. Read more about Insecure data storage [here.](https://owasp.org/www-project-mobile-top-10/2014-risks/m2-insecure-data-storage) More about shared preferences [here.](https://developer.android.com/training/data-storage/shared-preferences)

Shared preferences folder lives in data>data>package_name>shared_prefs, so here data>data>jakhar.aseem.diva>shared_prefs holds the shared preferences. After navigating into that directory I saw a *WebViewChromiumPrefs.xml* file inside it.

![Untitled](/diva_APK/6.png)

Then I opened *WebViewChromiumPrefs.xml file*  

![Untitled](/diva_APK/7.png)

I got two values from here yes and 447211456. I entered this as username and password in the app and got the toast message saying "3rd party credentials saved successfully!”. Also the values that I entered is also stored as another xml file the shared_prefs folder!

![Untitled](/diva_APK/8.png)

![photo_6278190504336275175_y.jpg](/diva_APK/9.jpg)

---


4.**Insecure data storage Part-2**

As before I went to the *InsecureDataStorage2Activity* class and found that it is using SQLite database to store the credentials. Using adb I went to data>data>package_name>database as all the databases are stored here and there were many files in it.

![Untitled](/diva_APK/10.png)

To know which database the credentials is stored I got back into the *InsecureDataStorage2Activity* class and it is stored in database named *ids2.*

![Untitled](/diva_APK/11.png)

I googled how to open a database file in the terminal itself and got to know *sqlite3* would help in this case.I opened the database and viewed the data in it and yeah it is being stored here!

![Untitled](/diva_APK/12.png)

![photo_6282532372675147580_y.jpg](/diva_APK/13.jpg)

---


5.**Insecure data storage Part-3**

In the *InsecureDataStorage3Activity* class we can see that credentials are stored inside a file whose name consists of the words *info* and *tmp.* I navigated inside data>data>package_name and got to see a file name with *info* and *tmp* in it and opened it and yeah the credentials are stored there!

![Untitled](/diva_APK/14.png)

![photo_6282532372675147609_y.jpg](/diva_APK/15.jpg)

---


6.**Insecure data storage Part-4**

In the source code we can see two method calls `getExternalStorageDirectory` and `getAbsolutePath` which suggests that the file containing the credentials is being stored in the external storage of the device with the file name *.uinfo.txt.* We can also see that the file name is starting with “ . “ so its a dot file and dot files are hidden files.

![Untitled](/diva_APK/16.png)

![Untitled](/diva_APK/17.png)

I went to the sd card directory and checked for hidden files in it using *ls* command with -*a* flag in it. And yeah it is being stored in the this directory with the file name *.uinfo.txt*

![Untitled](/diva_APK/18.png)

---


7.**Input validation issues Part-1**

' or 1=1 —

when we input a username that we get from the source code we can see that the credentials are being shown.

---


8.**Input validation issues Part-2**

![Untitled](/diva_APK/19.png)

by using file:///path_to_any_file will be able to fetch any data and display it in the app

---


9.**Access control issues Part-1**

As in the challenge description, we need to access the API credentials outside from the app.

In the source code, from the *AccessControlActivity* I **could understand that its redirecting to another activity which contains the credentials.

![Untitled](/diva_APK/20.png)

So i looked for all activity in the app using *adb shell.* Then I saw bunch of activities in it.

![Untitled](/diva_APK/21.png)

`dumpsys package | grep -i jakhar.aseem.diva | grep Activity` - to show all the activities in the app.

`dumpsys - system services`.

As for this challenge i tried opening the *APICredsActivity*  and yeah the app is redirecting to the activity where API credentials are!

![Untitled](/diva_APK/22.png)

`am start -n jakhar.aseem.diva/.APICredsActivity`

![photo_6300871139999726883_y.jpg](/diva_APK/23.jpg)

---


10. **Access control issues Part-2**

In the source code we can see that the intent passes a parameter which is of boolean value. And also the parameter is added to *strings.xml* file, in which we can see the actual parameter name.

![Untitled](/diva_APK/24.png)

In *string.xml* file we can see that *check_pin* is the actual name of the parameter.

![Untitled](/diva_APK/25.png)

Now I checked for the activities present to intent to it.

`dumpsys package | grep -i jakhar.aseem.diva | grep Activity`

![Untitled](/diva_APK/26.png)

Googled and found out that *--ez extra_key extra_boolean_value* flag can be used to pass boolean value to intent, so first tried out check_pin as the parameter name and 1 as the boolean value and it didn’t work and tried 0 as the boolean value and yeah its showing us the API credentials!

`am start -n jakhar.aseem.diva/.APICreds2Activity --ez check_pin 0`

![Untitled](/diva_APK/27.png)

![Untitled](/diva_APK/28.png)

---


11. **Access control issues Part-3**

In this we needed to access the private notes from outside the without a password. So in the source code I was able to see that a *cursor* is being created. Cursor contain the query request to be showed in the UI.

![Untitled](/diva_APK/29.png)

In the query here we can see that it requests something from *CONTENT_URL* in the *NotesProvider* class. Examining the class I was able to find that CONTENT_URI holds the URI of the content provider.

![Untitled](/diva_APK/30.png)

I tried to run a query accessing this URI and yeah its showing all the private notes!

![Untitled](/diva_APK/31.png)

---


12. **Hardcode2Activity**

In the source code we can see that a method named *access* is being called inside the if condition that checks for the password. This *access* method actually is declared in the *DivaJni* class which is the java class of the *JNI*.

![Untitled](/diva_APK/32.png)

![Untitled](/diva_APK/33.png)

The method *access* passes a parameter *str* which hold the vendor key. I ran *strings* command on the *libdivajni.so* native library file to find the value of the variable *str* which is **in the *cpp* file of JNI and got many strings in it.

![Untitled](/diva_APK/34.png)

Tried possible matching vendor key in the app and yeah the second string gives the access!

![Untitled](/diva_APK/35.png)

I analyse this also using ghidra. I located the function and checked for the string that is being compared and yeah I am able to access with that string!

![Untitled](/diva_APK/36.png)

![Untitled](/diva_APK/37.png)

---

  
13. **Input validation issue - Part 3**

> **memory corruption vulnerability**

To overflow the memory I tried the maximum input that can be entered in the edit text box and yeah its crashing the app!

![Untitled](/diva_APK/38.png)

Further investigating in the source code of the app we can see that there is a method `initiateLaunchSequence` being passded inside a *if condition*. Then I checked the method and found it was inside the java class of the *JNI*, a string parameter was also being passed inside it. So I analysed the app using ghidra then we can see that a string is being compared here.

![Untitled](/diva_APK/39.png)

![Untitled](/diva_APK/40.png)

And yeah trying that string gives us the toast message!

 

![Untitled](/diva_APK/41.png)

Hey, if you've made it this far, great! I'm proud of you. You're probably itching to dive deeper into the world of Android security! No? Then I hope you achieve what you're aiming for. And yes? I've been there. Want to keep the momentum going? I've put together a couple of resources that helped me on my journey. Check them out [here](https://xbr01.github.io/xbr-website/posts/resources/)

