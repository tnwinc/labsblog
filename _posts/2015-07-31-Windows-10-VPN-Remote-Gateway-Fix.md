---
title: "Windows 10 VPN Remote Gateway Fix"
layout: post
tags: [windows10, vpn]
published: false
author: David Carroll
mail: "david.carroll@websocially.com"
summary: Commandline work around to configure VPN connection to use local network for internet on Windows 10.
---

For those of you using Windows 10, you are likely unable to access the internet via your local network while connected to the VPN.  This is typically easy to resolve by following the instructions in this [article](http://www.nextofwindows.com/how-to-use-local-internet-connection-to-access-internet-while-still-connected-with-vpn).

However, several people have noticed the Properties button on the Networking tab is not responding in Windows 10.  

![Networking-Tab-Properties](https://slack-files.com/files-pub/T024SNHE3-F08EY2C2V-f3381e7d10/01-demo-vpn-networking-tab.jpg)

This appears to be a [known issue](https://social.technet.microsoft.com/Forums/en-US/af1cce20-ae21-4e89-bebc-11dc17becea5/no-access-to-internet-protocol-v4-or-v6-in-10049?forum=WinPreview2014Feedback#ec165fc5-f1ae-4e5e-95de-88b1ecbb2cac) and we've not found any instructions on resolving this yet.

In the meantime, you can configure this using the PowerShell instructions below:

From PowerShell, type:

    Get-VpnConnection

![Get-VpnConnection](https://slack-files.com/files-pub/T024SNHE3-F08EZ388M-a1037bfed5/01-get-vpnconnection.jpg)

If SplitTunneling is set to _False_, take note of the _VPN name_ and enter the following command:

    Set-VpnConnection "Demo VPN" -SplitTunneling 1

_Note: Replace "Demo VPN" with your VPN name._

![Set-VpnConnection](https://slack-files.com/files-pub/T024SNHE3-F08EZ0U5P-3c26ad1fba/01-set-vpnconnection.jpg)

Verify Split Tunneling is now set to True.

This should resolve your issue