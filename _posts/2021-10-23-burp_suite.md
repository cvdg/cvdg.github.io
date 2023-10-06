---
layout: post
title: "TryHackMe - Burp Suite"
date: 2021-10-23T18:00:00+02:00
categories: ["TryHackMe"]
tags: ["thm"]
---

| Key   | Value
| ----: | :--------
| Room  | [rpburpsuite](https://tryhackme.com/room/rpburpsuite)
| Date  | 2021-10-14
| User  | [wastebasket](https://tryhackme.com/p/wastebasket)

## Task 1: Intro

## Task 2: Installation

* If you'll be installing Burp (as it's commonly referred to) from scratch, you'll need to first visit this link: https://portswigger.net/burp/communitydownload

* Once you've reached the Port Swigger downloads page, go ahead and download the appropriate version for your operating system

* Burp Suite requires Java JRE in order to run. Download and install Java here: https://www.java.com/en/download/
  Once you've got everything setup move onto our next task, Gettin' [CA] Certified!

## Task 3: Gettin' \[CA\] Certified

* First, let's go ahead and launch Burp. We can do this on Kali via the icon on the left side. In the image below   
  it's the seventh icon from the top on the left-hand side. If your Kali desktop doesn't look like the screenshot 
  below, click on 'Applications' and type in Burp Suite. Click on the Burp Suite icon that appears.
* Launch Burp!
* Once this pops-up, click 'Temporary project' and then 'Next'.
* Next, we'll be prompted to ask for what configuration we'd like to use. For now, select 'Use Burp defaults'.
* Finally, let's go ahead and Start Burp! Click 'Start Burp' now!
* You'll now see a screen that looks similar to this:
  Since we now have Burp Suite running, the proxy service will have started by default with it. In order to fully 
  leverage this proxy, we'll have to install the CA certificate included with Burp Suite (otherwise we won't be 
  able to load anything with SSL). To do this, let's launch Firefox now!
  
  
* Navigate to the following link to install FoxyProxy Standard: Link
  Go ahead and install this now!
* Next, we'll move onto adding the certificate for Burp!
* With Firefox, navigate to the following address: http://localhost:8080
* Click on 'CA Certificate' in the top right to download and save the CA Certificate.
* Click on 'View Certificates'
* Next, in the Authorities tab click on 'Import'
* Navigate to where you saved the CA Certificate we downloaded previously. Click 'OK' once you've selected this certificate.
* Finally, select the following two options seen in this photo:
* Select 'OK' once you've done this. Congrats, we've now installed the Burp Suite CA Certificate!

## Task 4: Overview of Features 

* Which tool in Burp Suite can we use to perform a 'diff' on responses and other pieces of data?

`Comparer`

* What tool could we use to analyze randomness in different pieces of data such as password reset tokens?

`Sequencer`

* Which tool can we use to set the scope of our project?

`Target`

* While only available in the premium versions of Burp Suite, which tool can we use to automatically identify different vulnerabilities in the application we are examining?

`Scanner`

* Encoding or decoding data can be particularly useful when examining URL parameters or protections on a form, which tool allows us to do just that?

`Decoder`

* Which tool allows us to redirect our web traffic into Burp for further examination?

`Proxy`

* Simple in concept but powerful in execution, which tool allows us to reissue requests?

`Repeater`

* With four modes, which tool in Burp can we use for a variety of purposes such as field fuzzing?

`Intruder`

* Last but certainly not least, which tool allows us to modify Burp Suite via the addition of extensions?

`Extender`

## Task 5: Engage Dark Mode 

* With Burp Suite launched, let's first navigate to the 'User options' tab. 

* Next, click on the 'Display' sub-tab. 

* Now, click on the 'Look and feel' drop-down menu. Select 'Darcula'. 

* Finally, close and relaunch Burp Suite to have dark theme (or whichever theme you picked) take effect.


## Task 6: Proxy 

* Deploy the VM attached to this task!

  To complete this task you need to connect to the TryHackMe network through OpenVPN. If you're using the in-browser machine this isn't needed (but make sure you're accessing the machine and using Burp inside the in-browser machine).

* By default, the Burp Suite proxy listens on only one interface. What is it? Use the format of IP:PORT

`127.0.0.1:8080`

In Burp Suite, navigate to the Intercept sub-tab of the Proxy section. Enable Intercept

* Return to your web browser and navigate to the web application hosted on the VM we deployed just a bit ago. Note that the page appears to be continuously loading. Change back to Burp Suite, we now have a request that's waiting in our intercept tab. Take a look at the actions, which shortcut allows us to forward the request to Repeater?

`Ctrl-R`

* How about if we wanted to forward our request to Intruder?

`Ctrl-I`

* Burp Suite saves the history of requests sent through the proxy along with their varying details. This can be especially useful when we need to have proof of our actions throughout a penetration test or we want to modify and resend a request we sent a while back. What is the name of the first section wherein general web requests (GET/POST) are saved?

`HTTP History`

* Defined in RFC 6455 as a low-latency communication protocol that doesn't require HTTP encapsulation, what is the name of the second section of our saved history in Burp Suite? These are commonly used in collaborate application which require real-time updates (Google Docs is an excellent example here).

`WebSockets history`

* Before we move onto exploring our target definition, let's take a look at some of the advanced customization we can utilize in the Burp proxy. Move over to the Options section of the Proxy tab and scroll down to Intercept Client Requests. Here we can apply further fine-grained rules to define which requests we would like to intercept. Perhaps the most useful out of the default rules is our only AND rule. What is it's match type?

`URL`

* How about it's 'Relationship'? In this situation, enabling this match rule can be incredibly useful following target definition as we can effectively leave intercept on permanently (unless we need to navigate without intercept) as it won't disturb sites which are outside of our scope - something which is particularly nice if we need to Google something in the same browser.

`Is in target scope`

## Task 7: Target Definition 

* Before leaving the Proxy tab, switch Intercept to disabled. We'll still see the pages we navigate to in our history and the target tab, just having Intercept constantly stopping our requests for this next bit will get old fast.

* Navigate to the Target tab in Burp. In our last task, Proxy, we browsed to the website on our target machine (in this case OWASP Juice Shop). Find our target site in this list and right-click on it. Select 'Add to scope'. 

* Clicking 'Add to scope' will trigger a pop-up. This will stop Burp from sending out-of-scope items to our site map.

* Select 'Yes' to close the popup.

* Browse around the rest of the application to build out our page structure in the target tab. Once you've visited most of the pages of the site return to Burp Suite and expand the various levels of the application directory. What do we call this representation of the collective web application?

`Site map`

* What is the term for browsing the application as a normal user prior to examining it further?

`happy path`

* One last thing before moving on. Within the target tab, you may have noticed a sub-tab for issue definitions. Click into that now.

* The issue definitions found here are how Burp Suite defines issues within reporting. While getting started, these issue definitions can be particularly helpful for understanding and categorizing various findings we might have. Which poisoning issue arises when an application behind a cache process input that is not included in the cache key?

`web cache poisoning`

## Task 8: Puttin' it on Repeat\[er\] 

* To start, click 'Account' (this might be 'Login' depending on the version of Juice Shop) in the top right corner of Juice Shop in order to navigate to the login page.

* Try logging in with invalid credentials. What error is generated when login fails?

`Invalid email or password.`

* But wait, didn't we want to send that request to Repeater? Even though we didn't send it to Repeater initially via intercept, we can still find the request in our history. Switch over to the HTTP sub-tab of Proxy. Look through these requests until you find our failed login attempt. Right-click on this request and send it to Repeater and then send it to Intruder, too!

* Now that we've sent the request to Repeater, let's try adjusting the request such that we are sending a single quote (') as both the email and password. What error is generated from this request?

`SQLITE_ERROR`

* Now that we've leveraged Repeater to gain proof of concept that Juice Shop's login is vulnerable to SQLi, let's try something a little more mischievous and attempt to leave a devastating zero-star review. First, click on the drawer button in the top-left of the application. If this isn't present for you, just skip to the next question.

* Next, click on 'Customer Feedback' (depending on the version of Juice Shop this also might be along the top of the page next to 'Login' under 'Contact Us')

* With the Burp proxy on submit feedback. Once this is done, find the POST request in your HTTP History in Burp and send it to Repeater.

* What field do we have to modify in order to submit a zero-star review?

`rating`

* Submit a zero-star review and complete this challenge in Juice Shop!

## Task 9: Help! There's an Intruder! 

* Which attack type allows us to select multiple payload sets (one per position) and iterate through them simultaneously?

`Pitchfork`

* How about the attack type which allows us to use one payload set in every single position we've selected simultaneously?

`Battering Ram`

* Which attack type allows us to select multiple payload sets (one per position) and iterate through all possible combinations?

`Cluster Bomb`

* Perhaps the most commonly used, which attack type allows us to cycle through our payload set, putting the next available payload in each position in turn?

`Sniper`

* Download the wordlist attached to this room, this is a shortened version of the fuzzdb SQLi platform detection list.

* Return to the Intruder in Burp. In our previous task, we passed our failed login attempt to both Repeater and Intruder for further examination. Open up the Positions sub-tab in the Intruder tab with this request now and verify that 'Sniper' is selected as our attack type.

* Burp attempts to automatically highlight possible fields of interest for Intruder, however, it doesn't have it quite right for what we'll be looking at in this instance. Hit 'Clear' on the right-hand side to clear all selected fields.

* Next, let's highlight the email field between the double quotes ("). This will be whatever you entered in the email field for our previous failed login attempt.

* Now click 'Add' to select our email field as a position for our payloads.

* Next, let's switch to the payloads sub-tab of Intruder. Once there, hit 'Load' and select the wordlist you previously downloaded in question five that is attached to this task.

* Almost there! Scroll down and uncheck 'URL-encode these characters'. We don't want to have the characters sent in our payloads to be encoded as they otherwise won't be recognized by SQL.

* Finally, click 'Start attack'. What is the first payload that returns a 200 status code, showing that we have successfully bypassed authentication?

`a' or 1=1--`

## Task 10: As it turns out the machines are better at math than us 

* Switch over to the HTTP history sub-tab of Proxy. 

* We're going to dig for a response which issues a cookie. Parse through the various responses we've received from Juice Shop until you find one that includes a 'Set-Cookie' header. 

* Once you've found a request response that issues a cookie, right-click on the request and select 'Send to Sequencer'.

* Change over Sequencer and select 'Start live capture'

* Let Sequencer run and collect ~10,000 requests. Once it hits roughly that amount hit 'Pause' and then 'Analyze now'

* Parse through the results. What is the effective estimated entropy measured in?

`bits`

* In order to find the usable bits of entropy we often have to make some adjustments to have a normalized dataset. What item is converted in this process?

`token`

* Read through the remaining results of the token analysis


## Task 11: Decoder and Comparer 

* Let's first take a look at decoder by revisiting an old friend. Previously we discovered the scoreboard within the site JavaScript. Return to our target tab and find the API endpoint highlighted in the following request:

* Copy the first line of that request and paste it into Decoder. Next, select 'Decode as ...' URL

* What character does the %20 in the request we copied into Decoder decode as?

`Space`

* Similar to CyberChef, Decoder also has a 'Magic' mode where it will automatically attempt to decode the input it is provided. What is this mode called? 

`Smart Decode`

* What can we load into Comparer to see differences in what various user roles can access? This is very useful to check for access control issues.

`site maps`

* Comparer can perform a diff against two different metrics, which one allows us to examine the data loaded in as-is rather than breaking it down into bytes?

`words`

## Task 12: Installing some Mods \[Extender\]

* To start, let's go ahead and switch over to the Options sub-tab of the Extender tab. 

* Scroll down until you reach the 'Python Environment' section. Note, Burp requires the standalone edition of Jython.

* Download the standalone version of Jython from here: Link - I suggest saving this or moving it to your Documents folder

* Return back to Burp and hit 'Select file' under the Python Environment subsection for Jython standalone. Navigate to where you just downloaded this file and select it. 

* Burp is now set to go for installing extensions. Switch to the BApp Store sub-tab of Extender and look through the various extensions offered.

* Which extension allows us too bookmark various requests?

`Bookmarks`

## Task 13: But wait, there's more! 

* Download the report attached to this task. What is the only critical issue?

`Cross-origin resource sharing: arbitrary origin trusted`

* How many 'Certain' low issues did Burp find?

`12`

## Badge

[Badge: Burped](https://tryhackme.com/wastebasket/badges/burped)
