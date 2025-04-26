# Ubuntu-18.04-Disable-Netplan-switch-back-to-networking-etc-network-interfaces
Disable Netplan switch back to networking /etc/network/interfaces
		<p>The following procedure works for Ubuntu&nbsp;18.04&nbsp;(Bionic Beaver)</p>
<p>I.&nbsp;Reinstall the&nbsp;ifupdown&nbsp;package:</p>
<pre># apt-get update
# apt-get install ifupdown</pre>
<p>II.&nbsp;Configure your&nbsp;/etc/network/interfaces&nbsp;file with configuration stanzas such as:</p>
<pre>source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

allow-hotplug enp0s3
auto enp0s3
iface enp0s3 inet static
address 192.168.1.133
netmask 255.255.255.0
broadcast 192.168.1.255
gateway 192.168.1.1
# Only relevant if you make use of RESOLVCONF(8)
# or similar...
dns-nameservers 1.1.1.1 1.0.0.1</pre>
<p>III.&nbsp;Make the configuration effective (no reboot needed):<span id="more-3496"></span></p>
<pre># ifdown --force enp0s3 lo &amp;&amp; ifup -a
# systemctl unmask networking
# systemctl enable networking
# systemctl restart networking</pre>
<p>IV.&nbsp;Disable and remove the unwanted services:</p>
<pre># systemctl stop systemd-networkd.socket systemd-networkd \
networkd-dispatcher systemd-networkd-wait-online
# systemctl disable systemd-networkd.socket systemd-networkd \
networkd-dispatcher systemd-networkd-wait-online
# systemctl mask systemd-networkd.socket systemd-networkd \
networkd-dispatcher systemd-networkd-wait-online
# apt-get --assume-yes purge nplan netplan.io</pre>
<p>Then, you&#8217;re done.</p>
<p>Note:&nbsp;You&nbsp;MUST, of course, adapt the values according to your system (network, interface name&#8230;).</p>
<p>V.&nbsp;DNS Resolver</p>
<p>Because Ubuntu Bionic Beaver (18.04) make use of the DNS stub resolver as provided by SYSTEMD-RESOLVED.SERVICE(8), you&nbsp;SHOULD&nbsp;also add the DNS to contact into the /etc/systemd/resolved.conf file. For instance:</p>
<pre>....
DNS=1.1.1.1 1.0.0.1
....</pre>
<p>and then restart the systemd-resolved service once done:</p>
<pre># systemctl restart systemd-resolved</pre>
<p>The DNS entries in the ifupdown INTERFACES(5) file, as shown above, are only relevant if you make use of RESOLVCONF(8) or similar.</p>
<p>Src:&nbsp;https://askubuntu.com/questions/1031709/ubuntu-18-04-switch-back-to-etc-network-interfaces</p>
<div class="addtoany_share_save_container addtoany_content addtoany_content_bottom"><div class="a2a_kit a2a_kit_size_32 addtoany_list" data-a2a-url="https://tweenpath.net/ubuntu-18-04-disable-netplan-switch-networking-etc-network-interfaces/" data-a2a-title="Ubuntu 18.04: Disable Netplan switch back to networking /etc/network/interfaces"><a class="a2a_button_facebook" href="https://www.addtoany.com/add_to/facebook?linkurl=https%3A%2F%2Ftweenpath.net%2Fubuntu-18-04-disable-netplan-switch-networking-etc-network-interfaces%2F&amp;linkname=Ubuntu%2018.04%3A%20Disable%20Netplan%20switch%20back%20to%20networking%20%2Fetc%2Fnetwork%2Finterfaces" title="Facebook" rel="nofollow noopener" target="_blank"></a><a class="a2a_button_mastodon" href="https://www.addtoany.com/add_to/mastodon?linkurl=https%3A%2F%2Ftweenpath.net%2Fubuntu-18-04-disable-netplan-switch-networking-etc-network-interfaces%2F&amp;linkname=Ubuntu%2018.04%3A%20Disable%20Netplan%20switch%20back%20to%20networking%20%2Fetc%2Fnetwork%2Finterfaces" title="Mastodon" rel="nofollow noopener" target="_blank"></a><a class="a2a_button_email" href="https://www.addtoany.com/add_to/email?linkurl=https%3A%2F%2Ftweenpath.net%2Fubuntu-18-04-disable-netplan-switch-networking-etc-network-interfaces%2F&amp;linkname=Ubuntu%2018.04%3A%20Disable%20Netplan%20switch%20back%20to%20networking%20%2Fetc%2Fnetwork%2Finterfaces" title="Email" rel="nofollow noopener" target="_blank"></a><a class="a2a_dd addtoany_share_save addtoany_share" href="https://www.addtoany.com/share"></a></div></div>			<div class="category-and-tags">
				<a href="[https://tweenpath.net/category/administrations/](https://tweenpath.net/ubuntu-18-04-disable-netplan-switch-networking-etc-network-interfaces/)" rel="category tag">Administrations</a> <a href="https://tweenpath.net/category/configurations/" 				</div>
		
