---
layout: post
title: "TryHackMe - DNS in Detail"
date: 2021-10-13T13:00:00+02:00
categories: ["tryhackme"]
tags: ["thm"]
---

| Key   | Value
| ----: | :--------
| Room  | [dnsindetail](https://tryhackme.com/room/dnsindetail)
| Date  | 2021-10-13
| User  | [wastebasket](https://tryhackme.com/p/wastebasket)

## Task 1: What is DNS? 

What does DNS stand for?

`Domain Name System`

## Task 2: Domain Hierarchy

What is the maximum length of a subdomain?

`63`

Which of the following characters cannot be used in a subdomain ( 3 b _ - )?

`_`

What is the maximum length of a domain name?

`253`

What type of TLD is .co.uk?

`ccTLD`

## Task 3:  Record Types

What type of record would be used to advise where to send email?

`MX`

What type of record handles IPv6 addresses?

`AAAA`

## Task 4: Making A Request 

What field specifies how long a DNS record should be cached for?

`ttl`

What type of DNS Server is usually provided by your ISP?

`recursive`

What type of server holds all the records for a domain?

`authoritative`

## Task 5: Practical 

What is the CNAME of shop.website.thm?

`shops.myshopify.com`

What is the value of the TXT record of website.thm?

`THM{7012BBA60997F35A9516C2E16D2944FF}`

What is the numerical priority value for the MX record?

`30`

What is the IP address for the A record of www.website.thm?

`10.10.10.10`
