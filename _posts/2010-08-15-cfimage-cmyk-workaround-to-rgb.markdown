---
layout: post
title:  "CFIMAGE CMYK Workaround To RGB"
date:   2010-08-15 16:22:22
categories: cfimage cfml coldfusion imagemagick
---
I deal with a large amount of `CMYK` images for work. Some with Color Profiles and some without. So far `Coldfusion` hasn't been much help. Although it gets closer with each new release. Here is my workaround to convert `CMYK` images into `RGB` until Adobe lets us convert images with the `CFIMAGE` tag.

*Starting Image*

<img src="/images/cfimage-cmyk-rgb/bailey-cmyk.jpg" style="" />

Above is the starting image of my faithful coding companion Bailey. I converted it to `CMYK` and added the U.S. Web Coated (SWOP) v2 color profile in Photoshop. 

When you try and read this image in Coldfusion 8/9:

{% highlight  xml %}
<!--- Try to get image info --->
<cfimage action="info" source="#expandpath('.')#/images/bailey-cmyk-profile-800x600.jpg" structname="cmyk" />
{% endhighlight %}

It throws the following error:

{% highlight  plaintext %}
An exception occurred while trying to read the image.
Unsupported Image Type
{% endhighlight %}

This is where `Imagemagick` comes in. We can convert the image to `RGB` and remove the color profiles so we can continue. I chose to use a BASH script that I call with `CFEXECUTE`, but it appears to work calling it directly from `CFEXECUTE`. I had problems in the past with this so I have always done it this way, in case you were wondering.
{% highlight  plaintext %}
#!/bin/bash
infile=$1
outfile=$2
/usr/bin/convert $infile -strip -colorspace rgb -quality 100 $outfile
echo "Convert Finished"
exit 0
{% endhighlight %}

I pass this script the file I want to convert and where I want to create the new RGB version. The -strip command removes all the Photoshop meta data and the color profile. The `-colorspace rgb` command converts the image from CMYK to RGB. Finally the `-quality 100` sets the JPG quality setting for the new image, I created the original at 100 so I just kept it that way for the new one. (It can be adjusted to your personal preference or requirement from 1 to 100.) The new image will keep the same dimensions and resolution as the original image had to start.

Coldfusion Example:

{% highlight  xml %}
<cftry>
<!--- Try to get image info --->
<cfimage action="info" source="#expandpath('.')#/images/bailey-cmyk-profile-800x600.jpg" structname="cmyk" />
<cfcatch type="any">
<!--- If it fails convert to RGB and Strip Information with ImageMagick --->
<cfexecute name="#expandpath('.')#/img_to_rgb.sh" 
arguments="#expandpath('.')#/images/bailey-cmyk-profile-800x600.jpg #expandpath('.')#/images-convert/bailey-rgb-800x600.jpg" 
timeout="30" variable="msg" />
</cfcatch>
</cftry>
<!--- Image can now be read --->
<cfimage action="info" source="#expandpath('.')#/images/bailey-rgb-800x600.jpg" structname="cmyk" />
{% endhighlight %}
In the `Coldfusion` example I created here I first try to read the image information. If this fails I call the script to convert the image to `RGB` and remove the profile information. Then I read the file with `CFIMAGE` and it works. If that fails then you started out with a bum image to begin with.
If you aren't on OS X or Linux and want to call ImageMagick directly it would work like this :
{% highlight  xml %}
<cftry>
<!--- Try to get image info --->
<cfimage action="info" source="#expandpath('.')#/images/bailey-cmyk-profile-800x600.jpg" structname="cmyk" />
<cfcatch type="any">
<!--- If it fails convert to RGB and Strip Information with ImageMagick --->
<cfexecute name="/usr/bin/convert" 
arguments="#expandpath('.')#/images/bailey-cmyk-profile-800x600.jpg -strip -colorspace rgb -quality 100 #expandpath('.')#/images/bailey-rgb-800x600.jpg" 
timeout="30" variable="msg" />
</cfcatch>
</cftry>
<!--- Image can now be read --->
<cfimage action="info" source="#expandpath('.')#/images/bailey-rgb-800x600.jpg" structname="cmyk" />
{% endhighlight %}


*RGB File after Conversion*

<img src="/images/cfimage-cmyk-rgb/bailey-rgb.jpg" style="" />

So here is the new image of Bailey that is now RGB and all the Profile information removed. The coloring of the image changes slightly when you convert it to RGB from `CMYK`, but so far I haven't found a way to get it any closer. You can also resize and get the image info with `Imagemagick`, but I am doing that with `CFIMAGE`. In hopes that I can one day just comment this out and all the operations will work consistently. Maybe Adobe will fix that in the next release so this can all be done in `CFIMAGE`.
Hope this helps out others struggling with the same issue. This works well ( I have converted thousands of images) on Linux and OS X. I haven't tried it on windows, but I would assume it would work by either calling it directly or wrapping it in a BATCH file. If anyone tries it on Windows let me know.

*Links*

[ImageMagick](http://www.imagemagick.org)

<a href="/images/cfimage-cmyk-rgb/bailey-cymk-800x600.jpg">Bailey CMYK</a>

<a href="/images/cfimage-cmyk-rgb/bailey-rgb-800x600.jpg">Bailey RGB</a>

<a href="/images/cfimage-cmyk-rgb/code.zip">Code Example</a>
