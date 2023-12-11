### Welcome to my personal blog

## INIT: 2023/12/11
    Hello, I am Silverdev2482, well, it is the alias I go by online. This is my
first blog post, It is very word so sorry about that.

    I recently have gotten an eg25-g usb modem as a gift a few days ago, and have
it partially working but not completly working, Once I get it fully working with
SMS/MMS and calls on NixOS I plan on making a guide. I may also eventually make
that guide portable to other distros. I will roughly start from the beggining
of this journey before I started documenting it.

I ordered the antenna, model number: w6113b0100 from digikey($18), I haven't
checked any other sites in over a year but it is cheaper than sixfab($25) including
shipping. It is made by a brand called pulse electronics, includes a main,
diversity, and gps antenna. This makes it a perfect match for my eg25-g module.
This antenna is ment for use inside a device and appears to be fragile so it needs
an inclosure.

    I got my eg25-g module from ebay, they are usually cheaper on aliexpress($50-60)
although my dad doesn't like the idea of Chinese amazon so I got it on ebay. I see
at least 2 designs, you can tell by the placement of the u.fl ports, led, and 
presence/posiblility of a rf shield on the bottom. I see ones that don't have the
capiblility of having a rf shield, ones that do, and the same design were the
shield would be soldered on but it is missing. I don't think it matters much but
mine does have a shield. The ones that have shield are often watermarked:
"IoT Solutions Store" I have done little research but haven't found that company.
They could be very small and hard to find with the generic name, it could be an
alias, they could be bankrupt. I have no idea, if you know please tell me.

    Once I got the parts I hooked it up to my laptop, I was able to text but it
didn't always work. I could call but I couldn't hear anything, I think it may have
also hung up instantly but I don't remember. However I tried to connect to verizon
and couldn't, I'm not exactly sure what I was doing but I was using mmcli and
trying to connect with the apn vzwinternet, for some reason that didn't work.
I eventually came to using nm-connection editor, where it gave me a configuration
with a different apn name and a name of "Verizon 4G LTE Contract" I was unsure if
this was a good setup because the contract but it was the only preset with
Verizon in the name. I tried using it and it said "connected" in nmcli but I only
had an ipv6 address and couldn't route any traffic through it, ping -I interface
ipv6-address returned nothing.

    I struggled with this for a while, until I came on a guide from someone
called marcin in the pinephone modem sdk repo, It descibed how to fix an issue
where you get only an ipv6 address and how to route audio - something I needed
to do. well the link was dead and I thought that this person probably has
dissappeared from the internet, but I searched users in the pinephone matrix and I
found him, I sent him a message describing my problem, and lo and behold he
responded fairly quickly.

    I asked him about the guide and he said it wasn't
the best and the site it was hosted on was deleted. I told him about the issue
assuming it was a misconfigured modem, but I learned his solution was routing
ipv4 over ipv6, but my ipv6 connection was broken. Later we figured out his isp
was ipv6 only and the modem was fine. We pinpointed the problem of the
misconfigured apn, it turned out vzwinternet was the correct apn, changing that
in nm-connection-editor made it work.

    I still don't know why it didn't work
before, my best guess is that nm-connection-editor set some other setting right
where I only provided the apn. I will need to come back when I write the guide
to these things and figure out why they did or didn't work.

    The reason I'm writing this guide in the first place it to help those who
want to get a setup like mine. I will admit I have not done the RTM'ing I should
have done, because of that and not wanting to be a leach I decided I can at least
return a good guide to this community as payment for all my asked questions. If
you have problems feel free to open an issue on this repo for disscussion, I'll
try to help to my limited ability. Also, big thanks to marcin for all the help,
it wasn't much but it has gone a long way, try not to bother him, bother me first
but if we can't figure it out feel free to message him

PS: I have done 0 proof reading I hope this sounds slightly rational.

