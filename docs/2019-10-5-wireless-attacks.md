# Wireless Attacks

For this first post, I thought I'd share about some of the work I've been doing involving wireless attacks. I'd been asked to work on a demonstration of basic wireless attacks involving a WiFi Pineapple. For those that may not know what a Pineapple is, basically it's a device that allows people, both attackers and defenders, to conduct wireless security audits. Red teamers and pentesters can use this device to carry out wireless attacks in the hopes of denying wireless access to people, sniffing their traffic, or redirecting them to fake login portals in order to capture credentials to use to authenticate to the network. Blue teamers might use this as an auditing tool to asses the security of their wireless networks in the hopes of revealing any potential weaknesses that could be exploited.

The bells and whistles of Wifi Pineapples are well documented, and I'll add a link to their website at the end of this post in case someone is interested in experimenting with these types of devices. **DISCLAIMER: Do not experiment on any device that you do not personally own unless you have explicit *(and ideally written)* permission. It is illegal and jail is bad.**

## DeAuth / Evil-Twin attacks

This is an attack path that I've been experimenting with the most. There are tons of wireless networks the average person is in range of, and probably connects to, on a regular basis. Here's a shortlist of all the different places you come in contact with wireless networks:
    * Your house
    * Your friends/parents
    * Your favorite coffee shop
    * Airports
    * Walmarts
    * ...
Let's say I'm your neighbor, and we don't get along. Me, being the spiteful neighbor that I am, want to make you angry any way I can. To accomplish this, I've decided to disconnect your smart TV any time I know you're trying to watch your favorite shows/movies. My Pineapple can scan the area for SSIDs (the name you give your wireless network) and will report back all of the SSIDs it discovers, as well as the devices that are connected to those networks. Depending on how many devices are connected to the network, it could take some guess work before figuring out which device is the TV.

Once I've narrowed it down, I can deauth (or de-authenticate) your TV from your WiFi. And the best part is, I don't need to be on your network to accomplish this. I can also de-authenticate your TV whenever I want, as long as my Pineapple scans continue to find it. Pretty mean, right? While this is mainly just a nuisance, we can leverage this attack further for more malicious purposes, such as sniffing traffic from devices that aren't my own.

Cue the evil-twin attack. If I want to spy on what people are searching or doing on the internet near me, I'd want to execute a Man in the Middle attack (MitM). I want to place a device I control, in this case it would be the Wifi Pineapple, in between nearby devices and the internet. Let's say you, myself, and a bunch of strangers are hanging out at Starbucks, and I want to see what everyone in the Starbucks is doing on the internet. I'm going to set my Pineapple up to create a fake SSID (access point) that's name exactly the same as the legitimate Starbucks wifi and make sure it's broadcasting to nearby devices.

Next, I'm going to de-authenticate all of the devices connected to the legitimate Starbucks wifi, and I'm banking on most of the devices being configured to automatically connect to that wifi (because convenience). You might think your devices are connecting back to the real Starbucks wifi, but in reality they're connecting to my malicious wifi. Since my Pineapple is configured to connect to the internet, now most of the devices in Starbucks are connected to a device that I control, and I'm able to sniff their traffic. Whether you use a third-party module from the Pineapple or just fire up Wireshark/Burpsuite, as long as 
