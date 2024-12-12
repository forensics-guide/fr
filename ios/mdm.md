# Check for Mobile Device Management Profiles

Mobile Device Management (MDM) is a system commonly used by enterprises to control a fleet of mobile devices, and be able to, for example, issue configuration updates, install applications, or remotely wipe the data in case the device is lost. [Attackers have sometimes been seen abuse MDM](https://blog.talosintelligence.com/2018/07/Mobile-Malware-Campaign-uses-Malicious-MDM.html) in order to maintain control over their victims' phones, and install malicious applications.

The enrollment normally requires some manual interaction. An infection could happen, for example, if the attackers manage to obtain physical access to device (even for a short time), or by somehow social engineering the victims into enrolling themselves.

Similarly to malicious [iCloud accounts](icloud.md), MDM profiles should be visible in the settings of the device. If an MDM profile is installed on the device, opening "_Settings_", then "_General_", should reveal a "_Profile_" or "_Device Management_" menu option, typically below the "_iTunes Wi-Fi Sync_" and "_VPN_" menu options.

![](../.gitbook/assets/mdm.png)

_Image from BlackBag Technologies_

If the device owner does not recognize the MDM profile, and if the MDM profile does not appear to belong to an organization or company the device owner works with, it is possible that the device has been compromised.
