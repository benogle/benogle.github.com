---
layout: post
title: Sync your contacts between your Mac and LG phone
status: publish
type: post
published: true
archived: true
description: Steps to sync your LG Verizon phone's contacts with your mac and iPhone. Specifically your LG-VX8350.
keywords: mac, lg, verizon, vx8350, iphone, contacts, phonebook, address book, vcard
redirects:
- /2009/07/03/sync-your-contacts-between-your-mac-and-lg-phone/
---

So I broke down and braved the iPhone 3GS line at Apple San Francisco after work yesterday. Once
I got my shiny new 16GB phone home, I began the time-consuming process of setting it up to make
me feel warm fuzzies. This setup included attempts to transfer the numbers in my contacts from
my old LG-VX8350 phone to the shiny iPhone. Ramon at Apple told me just to connect via
bluetooth, then use iSync. Ok, easy enough. Wait, no.

It turns out that virtually none of the LG phones are supported in iSync. So I ended up using an
application called [BitPim](http://www.bitpim.org/) to pull the contacts, then I imported them
into my Mac's address book. When I plugged the iPhone into the laptop, they all were magically
synced. Cool.

So the more detailed steps are as follows:

* Download [BitPim](http://www.bitpim.org/) and drag it into the application folder.
* Turn on laptop bluetooth (strange icon on apple bar next to the wireless icon).
* On the VX8350, go to Menu -> Settings & Tools -> Bluetooth Menu and turn on the bluetooth
* Go through steps on laptop and phone to get the two connected.
* Open BitPim; have the phone auto-recognized.
* Click the Cellphone-with-arrow-to-the-right icon in BitPim to load the phone's phonebook data.
* Wait
* Eat some oatmeal
* Wait
* Select the contact names I want to keep and click the File -> Export -> vCards... menu item.
* In the save dialog, I selected 'Apple' in the 'dialect' list box and saved my new vcard with LG contacts
* Ate more oatmeal
* Open Address Book application and import my new vCard
* Plugged in iPhone

I hope this helps someone avoid entering them by hand.
