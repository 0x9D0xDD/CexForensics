CEX Forensics
=============

As people upgrade to the latest shiny new machine, many old platter hard disks are finding their way onto the second hand market on sites such as Ebay. I was curious, how well are these disks being erased before being resold?
Hard disks use magnetic platters to store data, structured into sectors. Your operating system will store files on that disk by making use of a reference table such as an inode table on Linux or FAT table on Windows. The table describes the location on disk of each file, so that it can be retrieved when required. Crucially when files are deleted, the reference to the file is deleted allowing it to be freed for another file. The data lies untouched on the disk until it is overwritten with a new file.  In lots of cases, the sellers appeared professional and claimed to have securely wiped the drive beforehand.
Luckily, there is another way of purchasing dodgy old hardware. Introducing CEX.



CEX is a UK trade-in-store where people can sell old Games, DVDs and computers. It has got a bit of a reputation of being very shady, mostly taking in stolen things for a few quid and putting a markup on it. This seemed perfect, how likely are they to properly wipe hard drives that they get through the door? After a little searching, I stumbled on an old 20Gb 2.5 inch SATA drive for the bargain price of 1 pound. The drive promptly arrived, safely packaged in an anti-static brown paper bag and envelope. 




I plugged the drive into my PC, and as I had anticipated the drive had been formatted. First job was to take an image of the drive, which was very straightforward to do using dd

dd if=/dev/sdb1 of=disk.img

This produced a nice 20gb image file that could be inspected. In parallel, I ran the disk through some open source tools such as (FTKImager), which identified the new partitions. A quick method of identifying files is to use a file carver like PhotoRec, it carves out known file extensions and dumps it in a fairly random order into folders. It allowed me to quickly identify the source of the drive, an Xbox 360. 

There were many system assets, wallpapers, animations and text artifacts pointing back to Microsoft domains.

http://compass.xboxlive.com/assets/2c/8b/2c8b8c08-3e75-4885-a956-859766eff01c.css?n=main.css

Many images were recovered from the drive, mostly thumbnails from the media center functions of the xbox.




Reviewing the domains list, I noted Bing.com so I constructed a grep on the disk to pull out all the searches that had been made.

strings /home/me/Forensics/disk.img | grep http://www.bing.com/search?

http://www.bing.com/search?q=123%20movies
http://www.bing.com/search?q=xmovies8+tv&qs=AS&pq=xmov&sc=8-4&cvid=C9D98AF6450945DFA0140FA0A307C150&FORM=QBRE&sp=1
http://www.bing.com/search?q=broutherhood%20free%20movie
http://www.bing.com/search?q=brotherhood+free+movie&FORM=AWRE
http://www.bing.com/search?q=broutherhood%20free%20movie
http://www.bing.com/search?q=brotherhood+free+movie&FORM=AWRE
http://www.bing.com/search?q=the+grand+tour&qs=AS&pq=the+gr&sk=AS3&sc=8-6&cvid=E15D2790EA2A4FD185AD7E11FD076CDE&FORM=QBRE&sp=4
http://www.bing.com/search?q=the+grand+tour+online+free&qs=AS&pq=the+grand+tour+o&sk=AS1&sc=8-16&cvid=DD70DAE807514C1188C78A33F15B7CE8&FORM=QBRE&sp=2
http://www.bing.com/search?q=megashare
http://www.bing.com/search?q=putlocker&qs=AS&pq=put&sc=8-3&cvid=A16DF815A2004BF2AD3938C112CD4A37&FORM=QBRE&sp=1

We can see that the owner was obviously keen on file sharing sites like put-locker.


