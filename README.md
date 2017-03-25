patch to impliment ignoring of MAC addresses which use bonjour sleep

Arpwatch is a horribly useful tool for detecting new/changed MAC addresses
on your network. While it's hideously old it's still quite useful or was
until I started using a 4th gen AppleTV. Then I started seeing endless MAC
address conflicts on my network due to Apple's "bonjour sleep proxy".

https://en.wikipedia.org/wiki/Bonjour_Sleep_Proxy

Debian, and I'm assuming Ubuntu have long had their own patch to arpwatch which
enables you to ignore devices but Centos/RHEL seems to lack this patch and I
couldn't get the Debian source to build on Centos.

So with a little assistance from a friend, here's my patch. It's clumsy in
that you need to specify the MAC address of your AppleTV in the code, but
it works.

Download the arpwatch srpm and install it. 

Edit your spec file in the 2 places you need, follow the format laid out in 
the spec file :

Patch20: arpwatch-ignore.patch
%patch20 -p1 -b .ignore

Download and edit the patch, place it in the SOURCES directory in your rpmbuild
structure. In the patch, you want to chance aa:bb:cc:dd:ee:ff to be the MAC 
of your AppleTV.

And then you should be able to rpmbuild -ba arpwatch.spec as normal.

This patch was tested on arpwatch-2.1a10, I'm assuming it'll work with later
updates, just change the path in the patch file to name the new source
directory name.


