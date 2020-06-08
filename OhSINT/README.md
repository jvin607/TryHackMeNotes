#OhSINT

```
Joe Vinyard | 20200607
```

#Task 1 OhSINT

What information can you possibly get starting with just one photo? (file available for download = WindowsXP.jpg)

##Run exiftool on image
```
ExifTool Version Number         : 11.99
File Name                       : WindowsXP.jpg
Directory                       : .
File Size                       : 229 kB
File Modification Date/Time     : 2020:06:07 16:10:15-04:00
File Access Date/Time           : 2020:06:07 16:10:46-04:00
File Inode Change Date/Time     : 2020:06:07 16:10:37-04:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
XMP Toolkit                     : Image::ExifTool 11.27
GPS Latitude                    : 54 deg 17' 41.27" N
GPS Longitude                   : 2 deg 15' 1.33" W
Copyright                       : OWoodflint
Image Width                     : 1920
Image Height                    : 1080
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1920x1080
Megapixels                      : 2.1
GPS Latitude Ref                : North
GPS Longitude Ref               : West
GPS Position                    : 54 deg 17' 41.27" N, 2 deg 15' 1.33" W
```
Of note, seems to be we have GPS coords
```
GPS Latitude                    : 54 deg 17' 41.27" N
GPS Longitude                   : 2 deg 15' 1.33" W
```
Which when converted into decimal we have: 54.294797 2.250369 which puts us in the North Sea

We also have 
```
Copyright                       : OWoodflint
```
Cursory search on duckduckgo yields the following results:
```
https://github.com/OWoodfl1nt/people_finder

https://oliverwoodflint.wordpress.com/
```
When we look at the github repo's README.md we gather the following information:
```
# people_finder

Hi all, I am from London, I like taking photos and open source projects. 

Follow me on twitter: @OWoodflint

This project is a new social network for taking photos in your home town.

Project starting soon! Email me if you want to help out: OWoodflint@gmail.com
```
Going to Twitter reveals:
```
From my house I can get free wifi ;D

Bssid: B4:5D:50:AA:86:41 - Go nuts!
```
Take BSSID and plug into WiGLE.net and we get SSID
```
UnileverWiFi
```
Taking a look at OWood's WP site we get the following:
```
Hey

Im in New York right now, so I will update this site right away with new photos!
```

1. What is the users avatar of?
```
cat (found on Twitter after github research)
```
2. What city is this person in?
```
London (based on Wigle.net research)
```
3. What's the SSID of the WAP he connected to?
```
UnileverWiFi (based on Wigle.net research)
```
4. What is his personal email address?
```
OWoodflint@gmail.com (based on github)
```
5. What site did you find his email address on?
```
github
```
6. Where has he gone on holiday?
```
New York (found via WP website)
```
7. What is this person's password?
```
pennYDr0pper.! (found in sourcecode for WP)
```
