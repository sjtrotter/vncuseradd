% VNCUSERADD(8) vncuseradd 1.0.0
% Stephen Trotter
% August 2022

# NAME
vncuseradd - add new LOGIN(s) with VNC capabilities

# SYNOPSIS
**vncuseradd** [*OPTION*] *LOGIN* [*LOGIN*]...\
**vncuseradd** [*OPTION*] -p *PASSWORD* *LOGIN* [*LOGIN*]...

# DESCRIPTION
**vncuseradd** utilizes the `newusers' utility to create LOGIN(s) in bulk (if they are not already created) and then assigns them a VNC display number based on their UID.

**vncuseradd** expects TigerVNC to be installed on the system, with the user configuration file stored at **/etc/tigervnc/vncserver.users**.

If a user is not yet created on the system, the *-p PASSWORD* option is mandatory to set a default system password for the user(s). **THIS SHOULD BE CHANGED** by the user(s) immediately upon login with the `passwd' utility.

VNC users created with this utility will have their default password set to *password*. **THIS SHOULD BE CHANGED** by the user(s) immediately upon login with the `vncpasswd' utility.

# OPTIONS
**-a**
: make the LOGIN(s) an admin account (adds user(s) to wheel group)

**-p *PASSWORD***
: Default password of the new user(s). For more than one *LOGIN*, *PASSWORD* will apply to all. This input method is **NOT SECURE** - have user(s) change ASAP with `passwd'

**-h**
: Display the help message.

**-s**
: Starts the VNC service(s) now (otherwise reboot or start manually)

**-v**
: Turn on debug mode.

**-V**
: Display version.

# EXAMPLES
**vncuseradd -ap *password* *newuser***
: Adds new user *newuser* with system password set to *password*, and VNC password set to *password*, and makes them an admin (adds to wheel group).

**vncuseradd -s *existuser1* *existuser2* *existuser3***
: Adds VNC capability to existing users, and starts their services now. You can also use bash expansion in this case, like this: *existinguser{1..3}*

# EXIT VALUES
**0**
: Successfully executed.

**64**
: TigerVNC config file not found.

**65**
: Unknown option passed to command.

**66**
: No *LOGIN* given as a positional parameter.

**67**
: No *-p PASSWORD* given for user not in system yet.

# BUGS
- -a will not have an effect if the user is already created, or if they are created and the home directory was already present before running this utility; i.e. this script will only add users to wheel if they are created by it completely.

# COPYRIGHT
Copyright 2022 Stephen Trotter. License GPLv3+: GNU GPL version 3 or later. <https://gnu.org/licenses/gpl.html>. This is free software: you are free to change and redistribute it. There is NO WARRANTY, to the extent permitted by law.