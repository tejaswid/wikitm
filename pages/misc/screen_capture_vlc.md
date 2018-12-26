---
title: Screen Capture Using VLC
keywords: VLC, screen capture, video
summary: "Explains how to do screen capture using VLC media player"
sidebar: misc_sidebar
permalink: misc_broadcasting_internet.html
folder: misc
---

1. Make sure the Desktop is clean and only the window of interest is in front. Arrange running applications such that the region being captured is not covered by another application, either behind or in front. Don’t overlap windows.

2. Launch VLC and position it such that it does not overlap with the region being recorded.

3. In VLC go to **Media >> Stream** and choose the **Capture Device** tab. Choose the mode as **Desktop**.

4. Set frame rate to 30.00 f/s or 60.00 f/s

5. In the same tab, click on **Show more options** and append the following text in the **Edit Options** field.
```
:screen-top=50 :screen-left=0 :screen-width=1200 :screen-height=700
```
Using this command we are specifying the record space 
(a rectangle) of the desktop. We specify the top left corner of the rectangle, its width and height in pixels. The position of the top left corner is meaured from the top left corner of the screen. Play around with these values to get the desired record space.

6. Click on **Stream**. The **Stream Output** dialog will pop up. The first section is the source section. Make sure it is set to **screen://**.

7. Click **Next** to open up the **Destination Setup** options. Click **Add**.

8. In the dialog box that opens up, give a proper location and filename. Make sure the extension is also correct, eg. **.mp4**.

9. Check the transcoding options and choose the appropriate option. The default works for an mp4. Leave the options section at default.

10. You are ready to go! Move any occluding windows aside and click on stream to start recording. Use the usual control buttons to start and stop capture.

## Troubleshooting
1. Errors related to codecs not being found: Try installing [https://www.codecguide.com/download_kl.htm](K-Lite codec pack). This usually comes with a lot of the common codecs and might solve the issue.

{% include links.html %}
