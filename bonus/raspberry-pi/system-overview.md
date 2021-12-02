---
layout: default
title: System overview
parent: + Raspberry Pi
grand_parent: Bonus Section
nav_exclude: true
has_toc: false
---

## Bonus guide: System overview
{: .no_toc }

Difficulty: Easy
{: .label .label-green }

To get a quick overview over the system status, I created [a shell script](resources/20-raspibolt-welcome) that is run as "message of the day" (motd) to be shown on login or on demand.

![MotD system overview](images/60_status_overview.png)

### Installation

This script will run as root, so please check it before blindly trusting me.

* Install necessary software packages

  ```sh
  $ sudo apt install jq net-tools netcat
  ```

* Download and inspect the script. Make sure to get the right script for the RaspiBolt version you run.

    ```sh
    $ cd /tmp/

    # RaspiBolt version 3 (current)
    $ wget https://raw.githubusercontent.com/raspibolt/raspibolt/master/resources/20-raspibolt-welcome

    # RaspiBolt v2 (deprecated)
    $ wget https://raw.githubusercontent.com/raspibolt/raspibolt/master/resources/20-raspibolt-welcome-v2 -O 20-raspibolt-welcome

    $ nano 20-raspibolt-welcome
    ```

* Install the script so that you can run it with `sudo raspibolt`

  ```sh
  $ sudo mv 20-raspibolt-welcome /usr/local/sbin/raspibolt
  $ sudo chmod +x /usr/local/sbin/raspibolt
  ```

### Configure "Message of the day" (optional)

If you want to show the welcome script every time you log in, you can set it as a "Message of the day" (MOTD):

* Link the welcome script into the motd folder

  ```sh
  $ sudo ln -s /usr/local/sbin/raspibolt /etc/update-motd.d/20-raspibolt-welcome
  ```

* In case the script runs into problems, it could theoretically prevent you from logging in.
  We therefore disable all motd execution for the "root" user, so you will always be able to login locally as "root" to disable it.

  ```sh
  $ sudo su
  $ touch /root/.hushlogin
  $ exit
  ```



You can now start the script with `sudo raspibolt` and it is shown every time you log in.

If the script is showing 'Public Not reachable' but you do have incoming connections and the blockchain is syncing, you might have a router that does not support NAT Loopback. Please check your node at https://bitnodes.earn.com, if it displays your node as available remove the # on lines 147 and 148 and put them before 149 and 150 by editing the file:

```sh
$ sudo nano /etc/update-motd.d/20-raspibolt-welcome
```
Both methods work, but the original method does not rely on third party applications and the earn.com method obviously does, but it is better than no working method at all.

------

<< Back: [+ Raspberry Pi](index.md)
