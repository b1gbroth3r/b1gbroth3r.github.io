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

Next, I'm going to de-authenticate all of the devices connected to the legitimate Starbucks wifi, and I'm banking on most of the devices being configured to automatically connect to that wifi (because convenience). You might think your devices are connecting back to the real Starbucks wifi, but in reality they're connecting to my malicious wifi. Since my Pineapple is configured to connect to the internet, now most of the devices in Starbucks are connected to a device that I control, and I'm able to sniff their traffic. Whether you use a third-party module from the Pineapple or just fire up Wireshark/Burpsuite, as long as a device isn't connected with an encrypted connection, you can see all of their traffic in plaintext.

You have successfully implemented a MitM attack at this point. Now that I've outlined what these attacks can do and how someone might go about it, I want to explain some easy but effective methods to mitigate these types of attacks.

To prevent against de-authentication and Man in the Middle attacks:
1. Don't allow your devices (phones, tablets, ipads, etc) to automatically connect to wireless networks, especially networks you don't trust or have control over. While an attacker can disconnect your devices from a network, if those devices don't actively attempt to reconnect to a network with the same name (legit or not) you can control and monitor what you're connecting to. While this removes the convenience of not having to manually connect to networks, you can ensure you're connected to the network you intended.
2. Whenever possible, connect to wireless networks that require authentication. WPA2 wireless authentication is preferred, as older authentication protocols have been broken over the years. If you have unlimited data, using your phone as a hotspot isn't a bad call when in an airport or coffee shop. Having access to a VPN is also a good approach. A virtual private connection will send your traffic through an encrypted tunnel, preventing someone from sniffing your traffic.
3. Only browse to websites that enforce HTTPS. This is becoming more and more common on the internet, especially on any site that handles personal data or login information. As more websites enforce encrypted sessions between their site and the clients connecting to it, the harder it will be for MitM attacks to capture website login credentials in plaintext, as well as other sensitive information.

That's all I have for now, but I might update this post in the next few weeks about how HTTPS works, what certificates are, and how an attacker might use their own certificate to decrypt encrypted HTTPS traffic and read the data in plaintext.

Links:
[Link to Wifi Pineapple](https://shop.hak5.org/products/wifi-pineapple)