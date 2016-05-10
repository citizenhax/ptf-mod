Shameless copy of:
The PenTesters Framework (PTF)

With the following updates/customization:

#Exploitation
	beef: fixed dependancy (ruby-dev, libsqlite3-dev
	HconSTF:updated after commands
	kingfisher: fixed very broken after_commands
	maligno: fixed dependancy
	responder: removed invalid launcher, updated after command to make Responder.py executable
	settoolkit: updated launcher to include seautomate & seproxy
	shellnoob: added launcher
	yersinia: installing via .deb package-> this tool hasn't been updated almost a year.
#Intel gathering
	dnsenume: updated for no-user prompt requierd
	masscan: updated launcher
	shell-storm-api: added launcher
	SimplyEmail: broken upstream
	smtp-user-email: added launcher
	udp-proto-scanner: added launcher
	yapscan: added launcher
#Password Recover
	crunch: added launcher
	hashcat: removed.  added oclhashcat instead
	johntheripper: fixed broken deps
	maskprocessor: fixed launcher
	princeprocessor: fixed launcher
	statsprocessor: fixed launcher
	dnscat2: need to fix this
	spraywmi: need to fix this later
	binwalk: fixed launcher
#vulnerability-analysis
	0trace.py: added launcher
	arachni: need to update launcher
	crackmapexec: fixed after_command
	lbd: broken
	sslyze: broken
	testssl: added launcher
	whatweb: broken
#Wireless
	aircrack-ng: fixed dependancies
	dot11decrypt: broken (need to find libtnis1 deb)
	kismet: fixed dependancies
	mana-toolkit: fixed dependancies

#Instructions:

For ubuntu, highly recommended to:
sudo apt-get install python-setuptools
sudo easy_install pip

Prior to install.

Check out the config/ptf.config file which contains the base location of where to install everything. By default this will install in the /pentest directory. Once you have that configured, move to running PTF by typing ./ptf (or python ptf).

This will put you in a Metasploitesque type shell which has a similar look and feel for consistency. Show modules, use <modules>, etc. are all accepted commands. First things first, always type help or ? to see a full list of commands.

For a video tutorial on how to use PTF, check out our Vimeo page here: https://vimeo.com/137133837

###Update EVERYTHING!

If you want to install and/or update everything, simply do the following:

./ptf

use modules/install_update_all

yes

This will install all of the tools inside of PTF. If they are already installed, this will iterate through and update everything for you automatically.

You can also show options to change information about the modules.

If you only want to install only for example exploitation tools, you can run:

./ptf
use modules/exploitation/install_update_all

This will only install the exploitation modules. You can do this for any module category.

#Modules:

First, head over to the modules/ directory, inside of there are sub directories based on the Penetration Testing Execution Standard (PTES) phases. Go into those phases and look at the different modules. As soon as you add a new one, for example testing.py, it will automatically be imported next time you launch PTF. There are a few key components when looking at a module that must be completed.

Below is a sample module

AUTHOR="David Kennedy (ReL1K)"

DESCRIPTION="This module will install/update the Browser Exploitation Framework (BeEF)"

INSTALL_TYPE="GIT"

REPOSITORY_LOCATION="https://github.com/beefproject/beef"

X64_LOCATION="https://github.com/something_thats_x64_instead_of_x86

INSTALL_LOCATION="beef"

DEBIAN="ruby1.9.3,sqlite3,ruby-sqlite3"

ARCHLINUX = "arch-module,etc"

BYPASS_UPDATE="NO"

AFTER_COMMANDS="cd {INSTALL_LOCATION},ruby install-beef"

LAUNCHER="beef"

###Module Development:

All of the fields are pretty easy, on the repository locations, you can use GIT, SVN or FILE. Fill in the depends, and where you want the install location to be. PTF will take where the python file is located (for example exploitation) and move it to what you specify in the PTF config (located under config). By default it installs all your tools to /pentest/PTES_PHASE/TOOL_FOLDER

Note in modules, you can specify after commands {INSTALL_LOCATION}. This will append where you want the install location to go when using after commands.

You also have the ability for repository locations to specify both a 32 bit and 64 bit location. Repository location should always be the x86 download path. To add a 64 bit path for a tool, specify X64_LOCATION and give it a URL. When PTF launches it will automatically detect the architecture and attempt to use the x64 link instead of the x86.

Note that ArchLinux packages are also supported, it needs to be specified for both DEBIAN and ARCH in order for it to be properly installed on either platform in the module

###BYPASS UPDATES:

When using traditional git or svn as a main method, what will happen after a module is installed is it will just go and grab the latest version of the tool. With after commands, normally when installing, you may need to run the after commands after each time you update. If you specify bypass updates to YES (BYPASS_UPDATE="YES"), each time the tool is run, it will check out the latest version and still run after commands. If this is marked to no, it will only git pull the latest version of the system. For "FILE" options, it is recommended to always use BYPASS_UPDATE="YES" so that it will overwrite the files each time.

###After Commands:

After commands are commands that you can insert after an installation. This could be switching to a directory and kicking off additional commands to finish the installation. For example in the BEEF scenario, you need to run ruby install-beef afterwards.  Below is an example of after commands using the {INSTALL_LOCATION} flag.
 
AFTER_COMMANDS="cp config/dict/rockyou.txt {INSTALL_LOCATION}"

For AFTER_COMMANDS that do self install (don't need user interaction).

###Automatic Launchers

The flag LAUNCHER= in modules is optional. If you add LAUNCHER="setoolkit" for example, PTF will automatically create a launcher for the tool under /usr/local/bin/. In the setoolkit example, when run - PTF will automatically create a file under /usr/local/bin/setoolkit so you can launch SET from anywhere by simply typing setoolkit. All files will still be installed under the appropriate categories, for example /pentest/exploitation/setoolkit however an automatic launcher will be created.

You can have multiple launchers for an application - for example Metasploit you may want msfconsole, msfvenom, etc. etc. In order to add multiple ones, simply put a "," between them. For example LAUNCHER="msfconsole,msfvenom". This would create launchers for both.

### Automatic Command Line

You can also just run ./ptf --update-all and it will automatically update everything for you without having to do into the framework

### IGNORE Modules or Categories

The "IGNORE_THESE_MODULES=" config option can be found under config/ptf.config in the PTF root directory. This will ignore modules and not install them - everything is comma separated and based on name - example: modules/exploitation/metasploit,modules/exploitation/set or entire module categories, like modules/code-audit/*,/modules/reporting/*

# ptf-mod
