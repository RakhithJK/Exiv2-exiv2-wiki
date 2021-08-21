The Commercial License was owned by Andreas.  After he left the project, the team decided to only offer GPLv2.0.

About 100 folks have contributed code since the project started in 2004.  We believe setting up a commercial license would involve at least the following steps:

1. Getting written permission from every contributor.
2. Separately hosting the code for the commercial version.
3. A legal contract.
4. A commercial model for the use of any revenue.
5. Possibly other complexity and consequences. 

The team is small (at 2021-08-21 it’s about 8 of us).  When this was discussed in 2018, there were only 3 of us.  We got legal advice about the existing commercial license and effectively that said:

1. The commercial license is a contract between Andreas and his licensees.
2. Team Exiv2 has no obligations to holders of a commercial license and can proceed with GPLv2.0.
3. Andreas has no commercial rights to any code contributed since he left the project in 2017.

So, Exiv2 is GPLv2.0 only and you must comply with the implied obligations.  

***

Somebody said there is a legal way in which you can use Exiv2 and be free from GPLv2.0 and that is to host the code on a server and access the code remotely.  You should consider extracting the metadata from an image and send only the metadata to the server.  You’ll want to do that to reduce the band-width.  A typical image is about 10mb, of which the metadata is usually about 5k-10k.  You’ll find the code in Robin's book as a useful starting point.   Image Metadata and Exiv2 Architecture:  https://clanmills.com/exiv2/book

Another suggestion is that executing the exiv2 command-line may be legal use of open source and free you from the obligations of GPLv2. So we suggest you develop/test/document an option —socket n for the exiv2 application to launch and wait forever for commands to arrive on the socket. Exiv2 is fast (100+ images/second). This will be very fast.

If you want to use any of these possibilities, you’ll have to verify that using exiv2 as described is legal.


