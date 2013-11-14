xrdp_dualmon
============

About
-------------------------------
  A *workaround* solution to enable dual monitor in the xrdp server when using Sesman and X11rdp.
  This is a *true* multi monitor ("multimon") solution, every display is independent and can maximize x-windows to fit fullscreen.
  Usage: Pass the "/span" argument when RDPing to the xrdp server. The solution is based on the following:
  * xrdp project - https://github.com/FreeRDP/xrdp
  * Xinerama - http://sourceforge.net/projects/xinerama
  * fake xinerama - http://home.kde.org/~seli/fakexinerama



Prerequisites 
-------------------------------
  * Screens are assumed to have the same resolution.
  * Only two screens are supported (this is what I needed), but this could easily be changed.
  * Dualmon ratio "threshold" is a const which is also tweakable (explained in detail later).
  * Update (29/05/2013) - It appears as if there's a resolution limitation, both screens' width combined should not exceed 2800px. That said, I got some good feedback from several hundred users on an xrdp server - looks like it works.

Installation
-------------------------------
  * Download xrdp source and replace xrdp/sesman/session.c with the file in this repo.
  * Compile and install xrdp with the xinerama dependency.
  * Compile and install fake xinerama:
      * Run gcc -O2 -Wall Xinerama.c -fPIC -o libXinerama.so.1.0.0 -shared
      * Replace /usr/lib/libXinerama.so.1.0.0 with the new file you've just created.

How it works
-------------------------------
  * The support of dualmon RDP experience to an xrdp server is achieved by "faking" two virtual screens.
  * When a user connects to an xrdp server using "/span", a wider window is sent to the xrdp server in client_info.
  * When width/height is larger then a certain ratio, it is safe to assume that this user is actually using two monitors.
  * At this point, and before the X11 session starts, we want to create the fake xinerama file, that creates two "virtual" screens.
  * In fact the two virtual screens are configured to be sized just like the physical ones - this creates the dualmon experience.
  * So not the most elegant solution, but it looks like it works just fine. Note that the RDP protocol *does* send a different header when "/multimon" is used, but I've decided to create a simpler version.

