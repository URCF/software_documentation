---
title: Rclone
permalink: /Rclone/
---

**Rclone**[1] syncs your files to cloud storage.

NOTE this is a manual process.

Installed Version
-----------------

**Rclone 1.56.3** is available on Picotte. Use the modulefile:

`   rclone`

Setup for Drexel OneDrive
-------------------------

The instructions follow [Rclone Documentation - OneDrive Setup](https://rclone.org/onedrive/), and [Rclone Documentation - Remote Setup](https://rclone.org/remote_setup/).

### Caution

Home directories on Picotte are limited to 64 GB. If you will be
downloading more than 64 GB of data from OneDrive, do it in your
research group directory.

### Prerequisites

You will need a computer with a browser to generate the *access token*
(string which grants Rclone access to your OneDrive). This computer will
need to have Rclone installed.

This could be Picotte itself, but will require the use an X11 server on
your PC: MobaXTerm on Windows,[2] or XQuartz on macOS.[3] This is a
laggy system: before starting Rclone setup, login to
<https://outlook.office.com/> with your Drexel account.

### Setup Procedure

On `picotte001`:

``` text
[juser@picotte001]$ module load rclone
[juser@picotte001]$ rclone config
```

Enter "`n`" to configure a new remote drive:

``` text
e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> n
```

Enter a name of your choice for this remote, e.g. "drexel":

``` text
name> drexel
```

Enter the type of remote storage. Use the number or the string in
double-quotes:

``` text
...
26 / Microsoft OneDrive
   \ "onedrive"
...
Storage> 26
```

Just hit Enter for the "OAuth Client ID" and "OAuth Client Secret" (i.e.
leave both blank).

``` text
OAuth Client Id
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_id>
OAuth Client Secret
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_secret>
```

Use "Microsoft Cloud Global" for the "national cloud region":

``` text
Choose national cloud region for OneDrive.
Enter a string value. Press Enter for the default ("global").
Choose a number from below, or type in your own value
 1 / Microsoft Cloud Global
   \ "global"
 2 / Microsoft Cloud for US Government
   \ "us"
 3 / Microsoft Cloud Germany
   \ "de"
 4 / Azure and Office 365 operated by 21Vianet in China
   \ "cn"
region> 1
```

Do not edit advanced config:

``` text
Edit advanced config?
y) Yes
n) No (default)
y/n> n
```

Do not use auto config because we will be using a "headless" (i.e.
without monitor, keyboard, or mouse) machine:

``` text
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine

y) Yes (default)
n) No
y/n> n
```

Now, on your PC (laptop, desktop, etc), or in an Xterm opened on
Picotte, where you have already installed Rclone:

``` text
$ rclone authorize "onedrive"
2021/10/05 14:32:21 NOTICE: Config file "/home/myname/.config/rclone/rclone.conf"
 not found - using defaults
2021/10/05 14:32:21 NOTICE: Log in and authorize rclone for access
2021/10/05 14:32:21 NOTICE: Waiting for code...
```

You should get a new browser window or tab opened to the Drexel
Microsoft login page. Login as usual, and you should be sent to a page
showing the message: "**Success!** All done. Please go back to rclone."

In your PC's terminal, you should see the token, which is a string
delimited by "{}". This string will span many lines of the terminal:
increase the terminal window size to see it all. Copy the token
including the starting "{" and the ending "}".

**SECURITY NOTE** DO NOT give anyone else this token. It allows full
access to your OneDrive.

``` text
2021/10/05 14:32:21 NOTICE: Got code
Paste the following into your remote machine --→
{"access_token":"xxxxxx.......
...","expiry":"....
-04:00"}
<---End paste
```

Go back to the Picotte terminal where you ran "`rclone config`", and
paste the token:

``` text
For this to work, you will need rclone available on a machine that has
a web browser available.

For more help and alternate methods see: https://rclone.org/remote_setup/

Execute the following on the machine with the web browser (same rclone
version recommended):

        rclone authorize "onedrive"

Then paste the result.

Enter a string value. Press Enter for the default ("").
config_token> {access_token":"xxxxx........
...","expiry":".....
-04:00"}
```

Next, specify the type of connection. **IMPORTANT** Use "OneDrive for
Personal or Business". Do not use any of the other types listed unless
you are certain.

``` text
Type of connection
Enter a string value. Press Enter for the default ("onedrive").
Choose a number from below, or type in an existing value
 1 / OneDrive Personal or Business
   \ "onedrive"
 2 / Root Sharepoint site
   \ "sharepoint"
 3 / Sharepoint site name or URL (e.g. mysite or https://contoso.sharepoint.com/sites/mysite)
   \ "url"
 4 / Search for a Sharepoint site
   \ "search"
 5 / Type in driveID (advanced)
   \ "driveid"
 6 / Type in SiteID (advanced)
   \ "siteid"
 7 / Sharepoint server-relative path (advanced, e.g. /teams/hr)
   \ "path"
config_type> 1
```

Rclone will prompt for the specific folder. Accept the default.

``` text
Drive OK?

Found drive "root" of type "business"
URL: https://drexel0-my.sharepoint.com/personal/myname123_drexel_edu/Documents

y) Yes (default)
n) No
y/n> y
```

At this final step, Rclone will print out the setup configuration for
confirmation. Enter "y" unless you wish to change it, and then "q" to
quit configuration.

``` text
----------------
[drexel]
type = onedrive
token = {"access_token":"xxxx..."}
drive_id = xxxxxxxx
drive_type = business
----------------
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
drexel               onedrive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

### Possible Operations

Rclone allows for copying directory structures between two endpoints,
and also syncing between two end points.

Please consult the official documentation.[4]

Example: Copy a directory from Picotte to OneDrive:

``` text
[juser@picotte001]$ module load rclone
[juser@picotte001]$ rclone copy /ifs/groups/myrsrchGrp/Data drexel:CopyOfData
```

This will create a directory named "CopyOfData" in the top level of your
OneDrive:

`    httpX://drexel0-my.sharepoint.com/personal/myname123_drexel_edu/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fmyname123%5Fdrexel%5Fedu%2FDocuments%2FCopyOfData`

Documentation
-------------

Consult official documentation: <https://rclone.org/docs/>

**IMPORTANT** [sync operations in Rclone run the risk of overwriting data](https://rclone.org/commands/rclone_sync/), e.g. replacing a new
version of a file with an old version. Please read the Rclone
documentation carefully.

References
----------

<references/>

[1] [Rclone website](https://rclone.org/)

[2] [Tips for Windows Users\#Graphical Display to Windows](/Tips_for_Windows_Users#Graphical_Display_to_Windows "wikilink")

[3] [Tips for macOS Users\#Graphical Display](/Tips_for_macOS_Users#Graphical_Display "wikilink")

[4] [Rclone Documentation](https://rclone.org/docs/)