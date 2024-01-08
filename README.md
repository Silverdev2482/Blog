## Welcome to my personal blog

### INIT: 2023/12/11
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
although my dad doesn't like the idea of Chinese amazon, so I got it on ebay. I see
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
where you get only an ipv6 address and how to route audio. well the link was dead
and I thought that this person probably has dissappeared from the internet, but I
searched users in the pinephone matrix and I found him, I sent him a message
describing my problem, and lo and behold he responded fairly quickly.

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

PS: I have done little proof reading so I hope this sounds slightly rational.

### 2023/12/12

I have done a little bit of proofreading and have better figured out markdown
syntax, I didn't do much of tinekering yesterday after I wrote the blog post,
these things are completly new to me.

I have sent some texts and they seem to work fine, including my dads phone, which
wasn't working for some reason, and I had my mom sent a text to me and waited
overnight and after plugging in the modem I recieved it just fine. I have found
that for some reason to recieve texts from someone I need to send them a text first,
this needless to say is a problem. I had an idea that disabling and reenabling USB
may mitigate this? although that idea hasn't been tested. 

The next thing I need to do is automatically route the audio from the modem to the
speakers. I would like it if the audio was routed from the default speaker and
microphone, even if it was change. On a different topic I wish there was a audio
control applet specifially designed for pipewire. I assume that the volume control
has some parity with pulse audio, I'm not really sure about that. pavucontol-qt
makes a mess of routing which is annoying in qpwgraph, in that regard pwvucontrol
is better but it can't change input volume or set the default sink/source which is
a deal breaker, I created an issue for that, maybe I'll learn rust and add those
features one day but that is far out of the scope of this blog/guide now and my
knowledge.

The problem I have run into, a basic rule I made to simply rename an output, which
could be inncorect, and is in /etc/wireplumber/main.lua.d/ isn't doing anything.
The guide on the nixos wiki mentions you can put stuff there but it isn't working
I also see a full wireplumber configuration directory in the nix store but I see no
options letting me add rules there. I'm also not sure if I could make a rule that
would change it with the default source/sink.

After a little doc reading I found out that wireplumber won't scan other directories
for configuration if a config directory is manually specified, which it is in the
systemd unit. maybe the wireplumber config options are under pipewire in NixOS
options?

I have designed a very basic "mount" for the modem and antenna, and as of writing
it is printing, the Freecad file should be in the files directory.
it however still need the following:
- A easy method of mounting to a laptop with little stuff left attached to the laptop
- A lid so the Modem and Antenna don't fall out
- A chamfer so it can easily slide into backpacks without catchaing.
- A method of holding the usb extension cable, so it doesn't get accidentally
pulled out, this will be adjustable also so it can used to catch different usb
extension cables, this may be unnessacy however as a higher quality extension
cable may not be liable to fall out, and I plan to get a better one as mine is
too long

I should also make and/or enable systemd units for chatty and gnome-calls so thoses
don't need to be started manually. I'm not sure if these already exist and simply
need to be made NixOS options to enable or they need to be made, I would suspect the
former rather than the later.

The final issue I see for now is that the a MMS dependeincy for chatty isn't package
I should be able to figure that out relativly easy.

So tldr to do:
- Figured out how to configure wireplumber on NixOS and automatically route the audio
via it, or find a different method
- If it works mitigate the USB audio issues on suspend by disabling and reenabling the
audio, either on recieving/sending a call or on wake from suspend
- Make or Enable systemd units for chatty and gnome-calls
- Find out why and fix why I can't recieve sms without previously sending someone a
message
- Finish the 3D printed case for the Modem.

Ps: I'm not good at markdown nor writing, and grammar/syntax corrections would be
apprecieated in a pr

### 2023/12/14

Sorry for missing the last blog, not much has happened, I printed the part and some
dimesions allow for too much wiggle room of the modem and squeeze the antenna, I
have adjusted the dimesions, along with moving the modem closer to the antenna to make
it smaller, I also added a few dependencies to my local nipkgs for chatty and I can
send MMS but the recipent doesn't recieve it. I also want to get gnome-contacts
working but can't figure it out. I haven't messed with wireplumber at all. so not much
has been happening lately.

### 2023/12/19

Still not much has happened now, I should have stated but my testing is pretty poor.
I am not having any problems with sending or recieving, even if a message hasn't been
sent before. I think I probably should have waited 30 more seconds to see if I have
still have problems recieving.

I have been daily driving this modem on my laptop without a phone. It the cell service
works ok but without a proper mount it is very inconvientent. My stupid laptop disables
the internal microphone if an external one is plugged in, even if it is the worst earbud
microphone known to mankind so this is a problem. Texting without mms works fine thought
but quickly checking stuff is annoying.

I have messed with the nix package previously and I got
the icons to work. I think libadwiatia is what was needed for that but I tried adding
mmsd and it appears to do nothing

### 2023/1/07

Hello I am back, I haven't done much over christmas and feel kinda bad about it, Just
stayed at home and wasn't being productive, but lets be optimistic, I can be productive,
So what have I done so far? I don't remember if I have said this but my tinkering in
nixpkgs is basically just adding and updating dependencies/programs. This has produced
no notable result so far. However my minecraft server has grown in popularity. It is
currently running on a i5 2nd generation computer with a 4tb 5400 rpm hard drive and 8
gigs of ddr3, my friend who got this one bought another friends computer with a 7th gen
i5, 256gb ssd, and 32gb of ddr4 ram. My friend bought it for 20$ but I think we stole it.
I said I would make a guide for nix-minecraft and transfer some docs, and this should be
a great time to do it, I'll make a tutorials directory in this blog, I also want to make
a dactly manuform with a track ball. Either using [this repo](https://github.com/bullwinkle3000/dactyl-keyboard)
or the cosmos keyboard configurator once that gets better. 
