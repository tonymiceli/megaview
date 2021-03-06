# megaview - IFF/GIF file/download view for the Commodore Amiga computer

Copyright (c) 1991, 1992, 1993 by Tony Miceli.  All rights reserved.

Written in 100% assembly language.

NOTICE:

  This program may be freely distributed, providing the program and its
supporting files remain intact and unchanged.  Commercial distribution of this
product is permitted, providing a suitable donation is made to the author.


URGENT BUGFIXED RELEASE!!

  Soon after I released version 2.40, it was brought to my attention that some
PAL machines couldn't open NTSC screens for some reason, so that problem
should be fixed now.  More importantly, though, was that when I finally got
access to a 1.3 machine again, I was shocked to find that the display was
always drastically centered to the left!!  That problem is also solved now
(in fact, it took about 2 seconds to fix as it was a VERY careless error which
was due to only one instruction!).

What is MegaView?

  MegaView is a small utility which allows you to view all standard amiga
graphic files (IFF ILBM format), Compuserve GIF 87a/89a format files, and
downloads of the above mentioned graphic files (see below).  All standard
amiga screen modes, up to and including AGA modes are supported.  MegaView 
has the ability to smoothly scroll bitmaps which are larger than the physical
screen size.  To scroll large bitmaps, just roll the mouse.  Non-HAM screens
are automatically faded on and off from a black background (AGA fading is VERY
smooth!).  When PAL screens are viewed on NTSC machines, the full height of 
the PAL picture is available via scrolling.  Click the left mouse button on the
screen to exit.

  As mentioned previously, MegaView is capable of viewing IFF/GIF downloads.
This facility is very useful for modem users as it allows viewing of the files
before they have been completely downloaded.  This allows you to spot the crap
files, (or the duplicate files some idiot uploaded to increase his/her UL:DL
ratio!) and abort the transfer before you have wasted the time for the complete
download.

  The download facility works by patching the DOS vectors for Open, Close and
Write.  The main program then puts itself into a waiting state (via the Wait() 
function).  When one of the patched DOS functions are called, the main program
will be signalled for action (via the Signal() function).  The program will
scan the buffer used with the Write function on newly opened files to determine
if the files are IFF/GIF or not.  If the file is IFF/GIF, then the program will
intercept all further Writes to the particular file and will continue viewing
the file piece-by-piece until the file has finished downloading, or the
download was cancelled.  As the program is mostly in a waiting state, it will
NOT slow the downloading at all.

  To cancel viewing of the current download, simply click the mouse on the
picture.  Note that this will not terminate the actual download -- to cancel
the actual download, you must use the comms program as normal.  To terminate 
the program while in download mode, click on the close gadget of the program's
window.


INSTRUCTIONS:

  This archive should contain the following files:

	MegaView          - The actual program.
	MegaView.info	  - Icon for workbench 2.0+ activation.
	MegaView_1.3.info - Icon recoloured for workbench 1.3 usage
	MegaView2.40b.doc - What you are reading now.

  Note: To use the icon MegaView_1.3.info, you will obviously need to rename
	it to MegaView.info after deleting the workbench 2 icon.  Also note
	that workbench 2+ users will never need the _1.3 icon.
 
  This program may be started from the CLI or workbench.  The program can
be started in the following two ways:

	CLI usage:	 MegaView [options] [filenames] [options] [filnames] ...

	Where [options] can be any of the above:

		-*	Request files.  This pops up a file requester, allowing
			you to select one or several files for viewing.  Note
			that this facility requires the asl or reqtools library
			to be present in the LIBS: directory.

		-c	Toggle screen centering on/off.  By default, centering
			is active.

		-d	Invoke download viewing mode.  MegaView will stay
			dormant with a window centered on the workbench screen,
			and will become active once a download is invoked.

		-f	Toggles screen fading on/off.  Default is on.

		-p	Toggle PAL-only mode.  When NTSC (ie. 200/240/400/480)
			height screens are viewed, PAL users have the ability
			to view them in the appropriate NTSC frequency.  This
			is useful for keeping the aspect ratios of the picture.
			However, some such pictures will look stretched when
			viewed in NTSC mode, so in order to view all files in
			PAL mode only, just use this swich.  By default, NTSC
			screens will be viewed in NTSC mode.  Note that this
			switch will have no effect on pre-workbench 2.0
			machines or NTSC machines.

		-v	Toggle vertical centering mode.  In order to center
			some screens vertically, they actually are shifted
			down on the display.  What happens in this case is that
			the screens behind the picture are made partially
			visible.  By default, these screens are not centered
			at all, as the result ends up looking worse.  If you
			do want them centered, however, just set this switch
			and all screens will be vertically centered.  The other
			case is when pictures slightly larger than the display
			are centered.  In this case, they have to be shifted
			upwards into the overscan area in order to be centered.
			Such pictures fully cover all background screens and
			are centered regardless of this switch.  Note that
			screen centering must be made active (which it is, by
			default) for this switch to take any effect.

	If no arguments are given, then the program will list a brief summary
	of available options.

	Workbench usage: There are two different methods for workbench
			 activation:
	
		a) Set MegaView as an icon's default tool.
		   ie. Click once on picture file's icon, select 'info' from
		       the workbench menu, change default tool to c:MegaView,
		       then select save.  Now, whenever the icon is activated,
		       MegaView will display the file.

		b) Hold down shift and single-click on all the files you would
		       like to view.  Then, while still holding down shift,
		       double-click on MegaView's icon.

	Available tooltypes:

		CENTERING=ON/OFF	Turns centering on or off.

		DL_MODE			Activates download viewing mode.

		FADING=ON/OFF		Turns screen fading on or off.

		AUTO_PAL		Activates auto-pal mode (See -p above).

		VERT_CENTERING=ON/UP	Turns vertical centering on, or only
					on for screens which are moved up in
					order to be centered (eg. overscan
					screens).  Same as -v switch above.

		AUTOREQUEST		Invokes a file requester.  Note that
					either asl or reqtools library must
					be present in LIBS: directory for
					this to work.

		Note.  Although DL_MODE, AUTO_PAL and AUTOREQUEST tooltypes do
		not require any arguments, workbench 1.3 insists there be one.
		I'm not 100% sure about this, but if this is the case, just
		append '=ON' to the end of the tooltype to keep workbench
		happy (MegaView will ignore any extension added, so it doesn't
		really matter what you add).  This problem does not exist on
		workbench versions 2 and up.


EXAMPLES:

	MegaView -cp df0:file.iff -c df0:file2.gif

		This will turn off centering (as default is on), and will 
		activate auto-PAL mode, before displaying file.iff.
		Centering is then turned back on, before displaying file2.gif.

	MegaView -*f -d

		This reveals a minor limitation in the command line parser.
		Take note that although 'f' appears after the '*', it will
		actually be performed before it.  This limitation only applies
		for the '*' switch, and all other switches are performed in
		correct order.  This command would thus be equivalent to the
		following:

	MegaView -f -* -d

		And this will turn off screen fading (default is on), then it
		will request a number of files with a file requester (just hold
		down shift while clicking on filenames to select multiple
		files) and will then invoke download mode.

	MegaView -d df0:file.iff

		Note that this will not invoke until after the file has been
		displayed.  This is quite logical as download mode is sort of
		a dead-end situation which can only be exited by quitting the
		program.

	If you want to use a filerequester and also have access to download
viewing when selected from the workbench, then I suggest you use the following
tool-list (you can add these via the 'info' command from the workbench)

		AUTOREQUEST
		DL_MODE

	This will always pop up a filerequester, then invoke download mode once
all files have been viewed.  If you just want download viewing, you can just
select cancel on the requester to enter download mode.  Or, if you only want
to view files, simply click on the close gadget once download mode begins.

IMPORTANT:

  A few points must be cleared, however, to understand how MegaView handles the
download.  As the program patches the DOS vectors, it can only begin viewing
the file or update the display when the Write() function is activated.  Most
communications programs, however, will use reasonably large buffers in memory
to speed up the transfers.  It is obvious that the comms program will only
perform the Write() function when its RAM buffer is full.  What this basically
means is that you are not going to see the file appear instantly as soon as
the download has been initiated, nor are you going to see a smoothly updating
display.  What will basically happen is that the file will start displaying
with the first disk access, and the screen will be updated with each later disk
access.

  For example, using NComm v2.0, Zmodem protocol at 2400bps.  NComm uses a 16Kb
buffer and hence dumps this buffer to disk approximately every 60 seconds.
Thus, with this particular setup, an IFF/GIF download will start to be
displayed about one minute after the actual download has begun, and later
portions of the file will be added to the display about every 60 seconds until
either the download has finished or is cancelled.

  This limitation of the program is not much of a problem though, because it
usually takes a couple of minute's worth of download for there to be enough of
the file present for the viewer to make accurate judgement of the file's
content.  Secondly, this approach does not slow the actual download.

  Another rather obvious limitation is that the program cannot view files that
are archived.  Thus, only downloads of raw files can be monitored.

  One side effect of the DOS-patching method is that when copying IFF/GIF files
from one place to another, MegaView will detect the creation of the destination
file and will display it.  I have dreamt up a few other methods of monitoring
downloads and I may implement them in future versions of the program, so watch
this space.

  Colour fading and scrolling facilities are not available in download mode for
obvious reasons.  When a scrollable picture is displaying in normal mode, you
may feel it to be necessary to position the mouse pointer at the top-left of
the screen, otherwise the display may jump.  In a future version, I will add
an option which will automatically position the mouse at the top-left to
overcome this.

  I spent quite a while optimising the GIF decompression routines.  For
example, a file which took 1 min, 3 seconds to load when I first wrote the code
now only takes 16 seconds!  This sounds good on paper (or monitor :), but with
certain highly compressed GIF images, it can take a little more time.  For this
reason, I have added a safety facility which will, in most cases, disable
viewing of the file currently being downloaded if the previous download buffer
was not decompressed when a new buffer is ready.  Note also, that GIF files
which have 256 colours will be converted to grayscale on pre-AGA machines.  On
any AGA equipped machines, the GIF files will be displayed in full-colour.
Also note that HAM8 mode is now fully supported too (again, only for AGA).
MegaView has no problems displaying GIFs which are in CompuServe's raster 
interlaced format (not to be confused with the Amiga's interlaced screen).
When an interlaced mode GIF is loaded, it will be loaded in a perculiar
manner -- The screen will be built up in four passes.  On the first pass, every
eigth row of the image will be displayed, then with the following three passes,
the remaining rows will be filled in.  This mode is great for viewing downloads,
as it gives you to have a rough impression of what the overall file looks like
long before all of the file has been received.


COMPATABILITY NOTICE:

  It has been brought to my attention that MegaView is NOT compatible with the
program JR-Comm.  This is NOT a bug.  JR-Comm does not seem to use the Write()
function during downloading (it uses some other method) and this thus makes
MegaView incompatible with it.  I may look into this further, but the only
alternative at the moment is to use a different comms program when you intend
to download any IFF/GIF files.

  MegaView has been shown to work with NComm and Term, and should work fine
when used with just about and other terminal program, except JRComm.  If you
do have it running under another terminal program other than these ones, I
would really appreciate it if you could let me know so I can make a list of
them.

  Remember, though, if the program doesn't initially seem to work, it may be
because the program is clashing with another program that patches itself into
the same dos vectors, for example.  Try to experiment with the order in which
such programs are launched in such a case.


THANX TO:

  Dale Baker for encouragement and testing, and for being a friendly sysop.
  Steve Boothman for suggestions and testing.
  Eddy Carroll whose's snoopglue.s source (distributed as part of snoopdos)
    gave me invaluable info on patching DOS vectors.
  Rodney Norton for suggestions.
  All other people who have made comments etc.


IMPROVEMENTS/BUG-FIXES FOR MEGAVIEW V2.40:

  o Fixed HEAPS of bugs to do with download mode.  It worked fine on my A500,
    but crashed :( on many other machines (to my horror!).
  o Full colour GIF support.
  o AGA compatible (ie. HAM8, 7 & 8 bitplane IFFs).
  o Fixed bug to do with filename for download mode.
  o Added a list of CLI switches and workbench tooltypes.
  o Added support for asl/reqtools file requesters.
  o Improved main window display.
  o Fixed bugs to do with bug-fixes from previous version for scrolling 8-<


FUTURE IMPROVEMENTS (DEPENDING ON THE RESPONSE I GET):

  o PowerPacked file compatibility.
  o Screen format requester for selecting different viewmodes.
  o JR-Comm support (this is a maybe, not a certainty...).
  o Work as a commodity under workbench 2.0+
  o Further options for CLI/workbench.
  o Autoscrolling windows for 2.0+ users in download mode.


  As I have made clear, this program is freeware, but is you use it and are
feeling generous, I would gladly accept any donations.  Also, if you find this
program useful, I would greatly appreciate any feedback.  Remember, is I don't
get any feedback, I can only assume that nobody is using this program, and I
will cease to continue developing it.

	  I can be contacted via the following postal address:

			      TONY MICELI
			      P.O. BOX 1083
			      CAMPBELLTOWN
			      NSW  2560
			      AUSTRALIA

	      The official support BBS for this program is:

	-=<--------------------------------------------------->=-

			     /X\idlands BBS

			   SYSOP - Dale Baker

			   FidoNET 3:713/610
			  AmigaNET 41:200/610

		   All speeds to 9600     V32/V42bis

		          Phone    02-824-8852

	-=<--------------------------------------------------->=-

	 This is the best place to contact me.  Otherwise, I may
	        be contacted at any of the below boards:

  	   Amitech (Cheeseburger) BBS		   02-544-1248
  	   AMY BBS				   02-607-4253
	   Enconn BBS				   02-524-1584

  I do call other boards in the same area, so you might be able to catch me
on any of them as well.

REMEMBER: If you find a bug, or have an idea for some improvement, I wanna know
	  about it!  Although, in order for a bug report to be of any practical
	  use, you must include information along the following lines:
	  
	  	o Machine specs (eg. model, size of RAM, processor, etc)
	  	o Nature of the problem (eg. crashes, does nothing)
	  	o Other programs used at the same time (comms program, in
	  						particular).
	  	o Let me know how you started the program up.
	  	o Any other relevant information would also help too.


  Cheers, have fun...

	Tony Miceli


PS. Anyone out there in need of a programmer who is about to complete a
    Bachelor of Science degree (computing major, of course, AND with a
    distinction average, but that's about it for my self-advertisement *8-)
