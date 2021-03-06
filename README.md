
XKB-SWITCH
===========
by J. Bromley, S. Mironov, Alexei Rad'kov

xkb-switch is a C++ program that allows to query and change the XKB layout state.
Originally ruby-based code written by J.Broomley.

* XKeyboard.cpp  Implementation for XKB query/set class
* XKbSwitch.cpp  Main program
* XKbSwitchApi.cpp The Vim API bindings

The C++ class has no special dependencies on anything outside of
X-related libraries, so it can be easily used with other software.

Xkb-switch is licensed under the GNU GPLv3, see COPYING for details.

Building
--------

Make sure you have package *libxkbfile-dev* (or *libxkbfile-devel* for Fedora) installed.

_CMake Hacker wanted: please help me to express this dependency in CMakeLists.txt_

To build the program manually, unpack the tarball and cd to source directory,
than type the following commands:

        $ mkdir build && cd build
        $ cmake ..
        $ make
        $ sudo make install

Usage
-----

	$ xkb-switch --help

	Usage: xkb-switch -s ARG            Sets current layout group to ARG
	       xkb-switch -l|--list         Displays all layout groups
	       xkb-switch -h|--help         Displays this message
	       xkb-switch -v|--version      Shows version number
	       xkb-switch -w|--wait [-p]    Waits for group change and exits
	       xkb-switch -W                Infinitely waits for group change
	       xkb-switch -n|--next         Switch to the next layout group
	       xkb-switch [-p]              Displays current layout group


*A note on `xkb-switch -x`*
Command line option `xkb-switch -x` has been removed recently. Please, use `setxkbmap
-query` or `setxkbmap -print` to obtain debug information.

VIM integration
---------------

Xkb-switch goes with a library libxkbswitch.so which can be called from
within Vim scripts like this:

        let g:XkbSwitchLib = "/path/to/libxkbswitch.so"
        echo libcall(g:XkbSwitchLib, 'Xkb_Switch_getXkbLayout', '')
        call libcall(g:XkbSwitchLib, 'Xkb_Switch_setXkbLayout', 'us')

See also [article in Russian](http://lin-techdet.blogspot.ru/2012/12/vim-xkb-switch-libcall.html)
describing complex solution.

Layout groups
-------------

xkb-group.sh can help you to manage layout groups. Just run it and send some
input at it's stdin every time you want to trigger layouts from primary to
secondary and back. For example:

	$ xkb-group.sh us ru
	switch # switch from us to ru or from current layout to us
	switch # switch from ru to us or from us to ru

	(from other terminal)
	$ xkb-switch -s de # switch to 'de' layout, change secondary layout to 'de'

	(back to terminal running xkb-group.sh)
	switch # switch from de to us
	switch # switch from us to de

Bugs or Problems
----------------

Admittedly, I only tested with a few different layouts that I used. If you find
any bugs let me know by submitting an issue or via grrwlf@gmail.com.

Regards,
Sergey.

