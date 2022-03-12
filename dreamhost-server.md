# Dreamhost Server

For running online studies, instead of AWS. It's our own server which serves our experiments, images, etc. publicly for people to do our studies.

It's at https://www.cognitivestudies.online.

We have the "Shared Unlimited" plan. As of June 2020, it's $119/year.
https://www.dreamhost.com/hosting/shared/

-----

https://panel.dreamhost.com/ enter the lab gmail and standard password to access.

Steps:

1. Set up a unique subdomain for each person, just so people can't accidentally modify or delete other people's files. For Alon Hafri, it's ah.cognitivestudies.online.
2. Set up a diff user account for each person (Alon's is alonhafri1). Password is standardly lastname-firstname, but this is not necessary.

-----

Here's an example of where you'd send the participant to (the link they should visit):
https://ah.cognitivestudies.online/tilt_shift/boundary_memory_study_v001.html

You should SFTP for file transfer.

Here are the domain settings:
Add [here](https://panel.dreamhost.com/index.cgi?tree=domain.manage)

* Do you want the `www` in your URL? Leave it alone: Both http://www.ah.cognitivestudies.online/ and http://ah.cognitivestudies.online/ will work.
* Run this domain under the user: [put their username]
* Web directory: /home/[username]/[initials].cognitivestudies.online
* Check PHP Priority Upgrade
* Check Extra Security
* Leave Passenger unchecked
* Leave CloudFlare unchecked

Here are the user settings:
https://panel.dreamhost.com/index.cgi?tree=users.users
You want:

* SFTP user - allows login via SFTP (SSH file transfer) for file transfers only.
* (or Shell)
* Disable FTP
* You can modify passwords here

You also want to to make an https secure SSL for subdomain. You just request a free "Let's encrypt" certificate for each subdomain (in the Domains > SSL/TLS Certificates sidebar). (It may take a day or two.)
To sign on:
`ssh [username]@cognitivestudies.online:/home/[username]`
For example:
`ssh alonhafri1@cognitivestudies.online:/home/alonhafri1`
