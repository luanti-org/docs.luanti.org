---
title: How to get a domain name for your server
aliases:
  - /How_to_get_a_domain_name_for_your_server
  - /how-to-get-a-domain-name-for-your-server
  - /server/domain
---

## What is DNS

_Skip to Step 1 if you already know about dynamic DNS._

DNS just means "Domain Name System". A domain name is a label that's easy for people to read or remember, which computers can use to look up an internet address (IP address).

For example, the domain "docs.luanti.org" tells your computer to go to an IP address like 89.188.9.43

If your game server is being run from your home, its internet address is likely to change over time, so having a domain name means you can give that out to friends/players and it won't change, even as your IP address changes. A domain name can also direct Web browsers to your server's webpage, if you have one.

If your server is for public use you can configure its minetest.conf file so it automatically announces itself to the public server list, this will keep everyone informed of your current IP address and website. In this situation a custom domain name for your sever becomes merely cosmetic. See [allowing external players to connect](/setting-up-a-server/#allowing-external-players-to-connect) for more details.

A domain that points to an address which may change is called "Dynamic DNS", and is what the rest of this page covers.

## Step 1: choose a domain

Just like "docs.luanti.org", your domain will have 3 parts separated with dots. You'll get to name the left-most part, and the rest you select from an available list. We'll use [FreeDNS](https://freedns.afraid.org/) because they have thousands of free domains to choose from.

[The full list you can choose from is here](https://freedns.afraid.org/domain/registry/)

If we type "minetest" into the search box for that list, we get:

![](/images/how-to-get-a-domain-name-for-your-server/FreeDNS_SearchResults.png)

In the status column, it shows that one of these domains is "public" and the other is "private". You can use either, the difference is that private domains allow the owner of the domain to [delete your subdomain if they find it offensive or slanderous](https://freedns.afraid.org/queue/explanation.php).

You don't have to use a domain containing "minetest", there are plenty of other domains available in that list, e.g. openblocks.com, various "Minecraft" ones etc., but our example Minetest server will be called "Secret Valley" and we will register _secretvalley.minetest.land_

Clicking on the minetest.land link takes us [here](https://freedns.afraid.org/subdomain/edit.php?edit_domain_id=1245383)

## Step 2: Create an account

If you aren't already logged in to FreeDNS, it'll ask you to log in.

Click "Setup an account here" and choose the free option.

## Step 3: Registering your subdomain

Once logged in, clicking a domain link brings up this dialog:

![](/images/how-to-get-a-domain-name-for-your-server/FreeDNS_AddSubdomain_Dialog.png)

We can leave Type, TTL, and wildcard as they are, and type in our server name. The Destination field has already been filled with your IP address.

Domain names are case insensitive and can only use letters, numbers, and hyphens - they cannot use spaces, so we type in "secretvalley":

![](/images/how-to-get-a-domain-name-for-your-server/FreeDNS_SecretvalleyExample_Dialog.png)

And click "Save!"

Now _secretvalley.minetest.land_ is pointing to my home internet connection, and thus my Secret Valley game server.

Much simpler than the scary and technical-looking user interface makes it appear!

## Step 4: Keeping your domain updated when your IP address changes.

You want your domain to be automatically updated if your ISP changes your IP address.

The program you use for this will depend on your operating system. There are many programs available for each OS:

[List of programs to keep your domain automatically updated](https://freedns.afraid.org/scripts/freedns.clients.php)

### A Windows example

The first program in the [list](https://freedns.afraid.org/scripts/freedns.clients.php) for Windows is [FreeDNS Update](http://www.techknowpro.com/freedns/).

After installing this, all I needed to do was enter my FreeDNS username and password then click the "Get Domain List" button. It logged into FreeDNS and obtained the domain:

![](/images/how-to-get-a-domain-name-for-your-server/FreeDNS_WindowsUpdaterExample.png)

The updater is now configured, it will sit in the Windows notification tray and regularly check the external IP address of the local network. If my IP address changes it will log into FreeDNS and update secretvalley.minetest.land to point to my new IP address.

My IP address only changes if the modem disconnects, which is pretty rare, so I set the Update Interval to be 20 minutes instead of 5.

(Note: the description for FreeDNS Update warns that if you use this app, you must enter your username in lowercase, and the password is case sensitive with a maximum length of 16 characters)

## Step 5: Let your Luanti server know its domain name

Edit your server's minetest.conf file, and set address to be your domain:

```conf
address = secretvalley.minetest.land
```

## Finished

You're done. FreeDNS provides additional features for domains that are outside the scope of this article, such as Web Forward, so you may wish to explore the site further.
