% VNCUSERADD(8) vncuseradd 1.3.1
% Stephen Trotter
% August 2022

# NAME
vncuseradd - add new LOGIN(s) with VNC capabilities

# SYNOPSIS
**vncuseradd** [*OPTION*]... *LOGIN* [*LOGIN*]...

# DESCRIPTION
**vncuseradd** uses `useradd' to create each LOGIN (if it is not already on the system) with a home directory copied from **/etc/skel**, then assigns each LOGIN the next available VNC display number, starting from *:10*.

**vncuseradd** expects TigerVNC to be installed on the system, with the user configuration file stored at **/etc/tigervnc/vncserver.users**.

**vncuseradd** must run as root. If invoked as an unprivileged user, it re-executes itself once via `sudo' (prompting for a password if necessary) and then runs every privileged step in the same root shell. When invoked from automation that is already root (for example, Ansible with `become: true', or `sudo vncuseradd ...'), the re-exec is skipped to avoid a double escalation.

For each LOGIN that does not already exist on the system, **vncuseradd** generates a random system password and immediately marks it expired, so the user is forced to choose a new one on first login. For each LOGIN that gets new VNC capability, **vncuseradd** also generates a random 8-character VNC password (the VNC protocol limits the effective key to 8 bytes).

Generated credentials are printed to standard output at the end of the run. They are shown exactly once: capture them before they scroll out of view. Use *-o FILE* if you need a persistent record on disk.

# OPTIONS
**-a**
: make the LOGIN(s) an admin account (adds user(s) to wheel group)

**-o *FILE***
: also write generated credentials to *FILE* (created with mode 0600). Without this flag, credentials are printed to stdout only.

**-h**
: Display the help message.

**-s**
: Starts the VNC service(s) now (otherwise reboot or start manually)

**-v**
: Turn on debug mode.

**-V**
: Display version.

# EXAMPLES
**vncuseradd -a *newuser***
: Creates new user *newuser* with a random expired system password and a random VNC password, and makes them an admin (adds to wheel group). Credentials are printed to stdout.

**vncuseradd -s -o /root/vnc-creds.txt *alice* *bob***
: Creates two users with random credentials, starts their VNC services immediately, and writes the credentials to */root/vnc-creds.txt* (mode 0600) in addition to printing to stdout.

**vncuseradd -s *existuser1* *existuser2* *existuser3***
: Adds VNC capability to existing system users (generating a random VNC password for each) and starts their services now. Bash expansion also works, e.g. *existuser{1..3}*.

# SECURITY
The system password is expired immediately with `passwd -e', so it is only valid for the very first login. After that, the user must set their own password.

The VNC password is not expirable by the protocol. Users should rotate it on first connect with the `vncpasswd' utility. The VNC protocol truncates the password to an 8-byte key, so generated VNC passwords are 8 characters long.

By default, credentials appear only on stdout, where they enter the terminal's scrollback and may be captured by terminal recorders (`script', `asciinema', tmux history). Treat the stdout output as sensitive. If you need a persistent record, pass *-o FILE* and protect or delete the file once the credentials have been distributed.

Generated passwords use an alphabet that excludes visually ambiguous characters (*0/O*, *1/l/I*) to reduce transcription errors.

# EXIT VALUES
**0**
: Successfully executed.

**64**
: TigerVNC config file not found.

**65**
: Unknown option passed to command.

**66**
: No *LOGIN* given as a positional parameter.

**68**
: Could not write credentials file specified with *-o*.

# BUGS
- -a will not have an effect if the user is already on the system; **vncuseradd** only adds users to the wheel group when it creates them itself.

# COPYRIGHT
Copyright 2022 Stephen Trotter. License GPLv3+: GNU GPL version 3 or later. <https://gnu.org/licenses/gpl.html>. This is free software: you are free to change and redistribute it. There is NO WARRANTY, to the extent permitted by law.
