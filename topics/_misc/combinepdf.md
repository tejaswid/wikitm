---
layout: page
title: Broadcasting internet
---
# Combining PDF files using ghostscript
Task: You want to combine multiple pdf files into one single pdf file. You dont want to upload it to a shady website.  
Solution: Use ghostscript on your computer.  
Works on both linux and windows.

## On Linux
Install ghostscript by typing the following in the terminal  
```
apt-get install ghostscript
```

Once done, use this command to combine pdfs  
```
gs -dBATCH -dNOPAUSE -sDEVICE=pdfwrite -dAutoRotatePages=/None -dAutoFilterColorImages=false -dAutoFilterGrayImages=false -dColorImageFilter=/FlateEncode -dGrayImageFilter=/FlateEncode -dDownsampleMonoImages=false -dDownsampleGrayImages=false -sOutputFile=fileAll.pdf file1.pdf file2.pdf file3.pdf file4.pdf
```

## On Windows
Download ghostscript from [their website](https://www.ghostscript.com/download/gsdnld.html).  
You can get the 32 bit or 64 bit AGPL version for your operating system.  
Install it wherever you want to.

Once done, you can find the executable at C:\Program Files\gs\gs9.53.3\bin  
You are looking for the gswin32c.exe or the gswin64c.exe  
The command to combine pdfs is the same as in linus. Just replace the gs at the beginning with the corresponding executable.  

Open up command prompt. Then navigate to the install folder and run the command.  
```
cd C:\Program Files\gs\gs9.53.3\bin
gswin64c.exe -dBATCH -dNOPAUSE -sDEVICE=pdfwrite -dAutoRotatePages=/None -dAutoFilterColorImages=false -dAutoFilterGrayImages=false -dColorImageFilter=/FlateEncode -dGrayImageFilter=/FlateEncode -dDownsampleMonoImages=false -dDownsampleGrayImages=false -sOutputFile=fileAll.pdf file1.pdf file2.pdf file3.pdf file4.pdf
```

Use the full path of the files and the location of the output encased in double quotes so that paths with spaces will work correctly. For example,  
-sOutputFile="C:\Users\My name\Path to folder\combined.pdf" "C:\Users\My name\Path to folder\file1.pdf" "C:\Users\My name\Path to folder\file2.pdf"