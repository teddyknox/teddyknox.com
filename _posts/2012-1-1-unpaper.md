---
layout: post
title: Cleaning up patchy book scans with Unpaper
date: 2010-12-03 10:20
categories: how-to
thumb: unpaper-thumb.jpg
tags: pdf, osx, command-line
excerpt: Save yourself toner by using this command-line program to remove the big black bars from the sides of pdf scans.
---

Set yourself up with

    brew install xpdf unpaper

And then use the following shell script (unpaper-pdf.sh):

{% highlight bash %}
#!/usr/bin/env sh
filename=$(basename $1 .pdf)
echo "Converting to ppm..."
pdftoppm $1 $filename
echo "Unpapering..."
unpaper -l $2 $filename-%06d.ppm output-%06d.ppm
echo "Converting to tiff..."
for i in `ls output-*.ppm`; do ppm2tiff $i $i.tiff; done
echo "Combining..."
tiffcp *.tiff all.tiff
echo "Converting to pdf..."
tiff2pdf -z -o $filename-unpaper.pdf all.tiff
echo "Cleaning up..."
rm $filename-*.ppm output-*.ppm *.tiff
{% endhighlight %}

Make it executable:

    chmod +x unpaper-pdf.sh

Our usage here becomes:

    ./unpaper-pdf.sh (single|double) textbook-scan.pdf
&nbsp;