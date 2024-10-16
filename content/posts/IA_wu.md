---
author: xbr
title: "Android APK Analysis: Part Two - Injured Android"
//subtitle: nah
date: 2023-11-08
Lastmod: 2023-11-08
//description:  
categories:
  - technical
tags:
  - android
  - apk
  - vulnerability
---

Lets dive into the second blog of the Android APK Analysis series
<!--more-->

<!-- # Injured Android apk --> 

--- 

### FLAG ONE - LOGIN

I analysed the source code using jadx, went to the corresponding activity class for any possible flag. There I was able to find two clues, "The flag is right under your nose.” made me go to the second clue ,"The flag is also under the GUI.”, and it suggested that the flag would be there in the code beneath it. So I scrolled through the code and yeah I was able to find the flag inside an *if condition* !

![Untitled](/injured_android_apk/1.png)

`F1ag_0n3` 

---

### FLAG TWO - EXPORTED ACTIVITY

In the challenge description it says that its possible to invoke exported activities without entering the main activity. So I took a look at the *AndroidManifest* file for the exported activities. Tried accessing all the activities which are exported and yeah I found the flag when I opened the *b25lActivity!*

`adb shell am start -n b3nac.injuredandroid/.b25lActivity` (n flag is for using the format 

`S3c0nd_f1ag`

---

### FLAG THREE - RESOURCES

In this activity I was told to enter the flag in the edit text box.

![Untitled](/injured_android_apk/2.png)

I analysed the source code using jadx and there was a *submitFlag*  method in which there was a string being validated by a *if condition* and the string was in string resources (strings.xml)

![Untitled](/injured_android_apk/3.png)

And yeah looking into that string gives the flag!

![Untitled](/injured_android_apk/4.png)

`F1ag_thr33`

---

### FLAG FOUR - LOGIN 2

As the previous challenge this also consists of a edit text box.

![Untitled](/injured_android_apk/5.png)

As always I analysed using jadx and found a *submitFlag* class in the source code in which there was a byte array being created but the code is obfuscated and I checked what is g()

 

![Untitled](/injured_android_apk/6.png)

the g() takes to a class in which there is a byte array created and initialized with value of base64 encoded string.

![Untitled](/injured_android_apk/7.png)

Decoding the base64 string and putting it in the edit text box in the app shows us that we have found the flag! 

![Untitled](/injured_android_apk/8.png)

`4_overdone_omelets`

---

### FLAG FIVE -EXPORTED BROADCAST RECEIVER

In the source code we can see that in *onCreate* method in which there is button OnClickListener in that a method named *H* is being passed with the instance of the current activity.

![Untitled](/injured_android_apk/9.png)

In the *H* method there is another method *F* being passed inside it, which is sending a broadcast message.

![Untitled](/injured_android_apk/10.png)

![Untitled](/injured_android_apk/11.png)

In above the *OnClickListener* that we examined before there is a reference to a class named *x.* when I accessed that class it was another activity named *FlagFiveReciver.* In *onRecieve* method inside it checks for the times its accessed and shows the flag in third try!     //lazy writing

![Untitled](/injured_android_apk/12.png)

an alternative way is to send broadcast receiver from *adb:*

`am broadcast -n b3nac.injuredandroid/.FlagFiveReceiver`


---


### FLAG SIX - LOGIN 3

```jsx
console.log("Script loaded successfully ");
Java.perform(function x() {
console.log("Inside java perform function");
var my_class = Java.use("b3nac.injuredandroid.k");

my_class.a.implementation = function(k){

console.log("argument is ",k);

var ret = this.a(k);

console.log(ret);

return ret;
}
	});
```

using this script i called a uncalled function

class *k* is loaded to the variable my_class

In the class *k* the uncalled function a being called and the with a parameter k and is printing the return value. which gives the flag

---


### FLAG SEVEN - SQLITE

In the onCreate method we can see that a method named *H* is being called in which there is a firebase database is being created. Moving on we can see that ContentValues is being created so some data is being stored in it and it can be accessed by content providers. So here there are two key value pairs being stored in it.

![Untitled](/injured_android_apk/13.png)

The key-value pairs are Base64 encoded and so decoding it i got:

`The flag hash!  — "VGhlIGZsYWcgaGFzaCE=”`

`2ab96390c7dbe3439de74d0c9b0b1767  — "MmFiOTYzOTBjN2RiZTM0MzlkZTc0ZDBjOWIwYjE3Njc=”`

Further brute forcing the hash value that I got `hunter2` 

The next key-value pair:

The flag is also a password! — "VGhlIGZsYWcgaXMgYWxzbyBhIHBhc3N3b3JkIQ==”

The value is stored in a String variable in another class which is being called in place of the value (*H* class).

![Untitled](/injured_android_apk/14_up.png)

![Untitled](/injured_android_apk/14.png)

The value is rot47 encrypted `"9EEADi^^:?;FC652?5C@:5]7:C632D6:@]4@>^DB=:E6];D@?”`

Decoding it gives a URL to the firebase https://injuredandroid.firebaseio.com/sqlite.json

From there we get the value `S3V3N_1` and boom! trying the value in the app gives the flag!

![Screenshot from 2023-09-19 21-48-57.png](/injured_android_apk/15.png)

![Untitled](/injured_android_apk/16.png)

In an alternative way using the hint in the challenge name we can access the database folder and look for the key value pairs and decode and use them to login!

`S3V3N_1`

`hunter2` 

---


### FLAG EIGHT - AWS

Have to configure the aws profile using aws configure and access the s3 bucket from the profile.

creds for creating the profile id and password will be in the strings.xml file.

`aws configure —profile` for creating the profile

was not able to create a profile

the flag was in that profile and should have been accessed using:

`aws s3 ls s3://injuredandroid --profile injuredandroid` (in this command the s3 bucket’s all the files are listed)

---


### FLAG NINE - firebase

worst write-up  

In the hint it was given that to use .json with the firebase url so on checking the code the url was not found so went on to the strings.xml and it was found there.

![Untitled](/injured_android_apk/17.png)

coming back to code there was a hint “Encoding” and there is string which was encoded in base64 which gave flags/ on decode.

![Untitled](/injured_android_apk/18.png)

 It seemed like an endpoint and tried it with the firebase url and yeah it showed the flag!

https://injuredandroid.firebaseio.com/flags/.json

the input is being decoded so we should make the flag base 64 encoded and submit in the text box.

![Untitled](/injured_android_apk/19.png)

"[nine!_flag]”

![Untitled](/injured_android_apk/20.png)

`[nine!_flag]`

---


### FLAG TEN - UNICODE

In this challenge we need to find out the email address which caused the unicode collsion  

the dotless i in the mail id is causing the mail id unicode collision, it was a vulnerability in github that while validating this mail the response was not sent to the right user

`John@Gıthub.com`

---

### FLAG 11 - DEEPLINKS

we should call the intent with the given in android manifest . Then only we can access the activity

vbox86p:/ # am start -W -a android.intent.action.VIEW -d "flag11://”

this checking for the command

![Untitled](/injured_android_apk/21.png)

`am start -W -a android.intent.action.VIEW -d "flag11://"` right , W-wait, a - action, action.View is the action that should be done. flag11:// is the scheme that the URI is set.

There is a hint in the activity “Find the compiled treasure”. So we can try seeing the binary.

We should decompile the apk and see the assets folder for the binary files.

Running the binary files gives the flag

![Untitled](/injured_android_apk/22.png)

![Untitled](/injured_android_apk/23.png)

`HIIMASTRING`

---


### FLAG TWELVE _ PROTECTED COMPONENTS

We need to pass two activity at the same time , one is protected one and the other one is the exported one.

![Untitled](/injured_android_apk/24.png)

It is cheaking for a string “totally_secured” , it is taking user input. There a class named ExportedPreotectedIntent which activity is exported. In that class also we can see there is a intent accepts. That class is also waiting for a string “access_protected_component”. The first intent is calling the next intent. the second intent passes the expected string and gets the activity.

---


### FLAG 13 rce

`am start -W -a android.intent.action.VIEW -d "flag11://binary"`

the process is getting a host and the host is not getting validated 

made a html file which access the files

`<html>
<p><a href="flag13://getprop">clickhere</p>
</html>` , getprop is used to get properties of device such as Sim Operator, IEMI, Android version and more

and the pushed it to the emulator and tried running the html in the browser and after which it should reditrect to the app and show the logs and it didnt work as intented 

not part of challenge: for the binary we can check if it takes as parms by using —help flag

The activity has a intent filter and has requirements of scheme:flag13 and host:rce 

![Untitled](/injured_android_apk/25.png)

so this is to be combined and put it in a hyperlink along with the parameters in the RCEActivity. In the activity we can see *binary* and *param* is the parameters checking for. And the files will be in files directory.

![Untitled](/injured_android_apk/26.png)

![Untitled](/injured_android_apk/27.png)

![Untitled](/injured_android_apk/28.png)

```jsx

// PoC

<html>
<p><a href="flag13://rce?binary=narnia.arm64&paramtestOne">Tesgtt one</p>
<p><a href="flag13://rce?binary=narnia.arm64&paramtestOne">Tesgtt two</p>
<p><a href="flag13://rce?binary=narnia.arm64&paramtestOne">Tesgtt threee</p>
<p><a href="flag13://rce?combined=Treasure_Planet">click here</p>
</html>
```

Here it access flag13 with parameters *rce* with architecture matching the system as value and *param* with the value command which shows in the webview.

After we run the hyperlink we can see it redirects to the RCEActivity.

we can run the binary in the file directory then it asks for parameter and we can see parameters using —help command while executing the binary. We can run the binary with the parameter to get the flag treasure.

![Untitled](/injured_android_apk/29.png)

---



### FLAG FIFTEEN - ASSEMBLY

when analysing the *native-lib.so* file using ghidra we can see the key as *win* when we xor it with the string we get `mad`. the byte array that is give in the challenge activity needs to be changed to string first and it gives something like ;;’  and we need to xor that string .

`win`

---

### FLAG SIXTEEN - CSP BYPASS

Content Security Bypass: It is used to detect XSS and injection attacks.

PoC : 

`<html>
<a href="http://b3nac.com/anything/">flag16</a>
</html>`

In this it check for the protocol and if its http then it changes it to https. It also checks if the login of origin is chrome and if it is it won’t be loaded. 

![Untitled](/injured_android_apk/30.png)

In the intent filter which we can see in the android manifest file has `pathPattern` in which we can input anything.

![Untitled](/injured_android_apk/31.png)

In the PoC we should send in http as there is a method to change it to https and in the manifest we can see it accepts https. 

Was not able to access the app using the PoC.

---

### FLAG SEVENTEEN - SSL PINNING BYPASS

This is a MITM attack challenge. A proxy is being set in the emulator and the wifi also being changed to the ip. we should use the flutter activity and pass any value in it. it can be using the frida hook - 

When we enter any link to the app in the flutter activity so we should hook the method which check the figerprint and gives the conditions true or false. so we should return true all the time. We should use burp suit to intercept the traffic and we can find the flag there.

`frida -u -f b3nca.injuredandroid -l flutter-plugin-ssl.js --no-pause`

---

If you found any alternative methods solving these challenges, I'd love to hear them. And if you spot anything in my writeup that could use a tweak or have ideas to make it better, just give me a shout! Always down to learn and improve.