xrdp_dualmon
============

About
-------------------------------
  A *workaround* solution to enable dualmon in the xrdp server. Based on:
  * xrdp project - https://github.com/FreeRDP/xrdp
  * fake xinerama - http://home.kde.org/~seli/fakexinerama

Installation
-------------------------------
  * Download xrdp source and replace xrdp_wm.c with the file under this repo.
  * Compile and install xrdp with xinerama dependency.
  * Compile and install fake xinerama.

Technical details
-------------------------------
  * Support dualmon RDP experience to an xrdp server is achieved by "faking" two virtual screens.
  * When a user connects to an xrdp server using "/span", a wider window is sent to the xrdp server in client_info.
  * When this width is larger then a certain ratio, it is safe to assume that the user is using two screens.
  * At this point, and before the x11 session starts, we want to create the fake xinerama file, that creates two "virtual" screens.
  * In fact the two virtual screens are configured to be sized just like the physical ones - this creates the dualmon experience.
  * So not the most elegant solution, but it looks like it works just fine. The RDP protocol *does* send a different header when "/multimon" is used, but I've decided to create a simpler version.
  
Other notes
-------------------------------
  * Screens are assumed to have the same resolution.
  * Only two screens supported (hard coded), but this could easily be changed.
  * Dualmon ratio "threshold" is a const defined in the code, and is also tweakable.
