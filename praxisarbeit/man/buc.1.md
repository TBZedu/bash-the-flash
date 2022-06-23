% buc(1) buc 1.1.0
% Conese Dillan
% June 2022

# NAME

buc - Bulk create new user accounts.

# SYNOPSIS

**buc** [*INPUTFILE*] [*ARGUMENTS*]

# DESCRIPTION

**buc**, short for "Bulk User Creation" creates multiple users based on an input file.

This script must be run as root.

# INPUT FILE

A file containing a list of users to create.

# OPTIONS

**-p *PASSWORD***
: Set a default password for all new users. The password must be changed after login.

**-v [*(D|I|W|E)*]**
: Verbosity level. If no level provided, the *D* is used.

# EXAMPLES

**buc ~/Documents/users -p s7kbaN2?20knBjpK9**
: Creates users based on the file ~/Documents/users, with password s7kbaN2?20knBjpK9.

**buc -v**
: Be verbose.

# EXIT VALUES

**0**
: Success

**1**
: Not run as root

**2**
: Invalid arguments

**255**
: Other error

# BUGS

My software has no bugs. It develops random features, the same way as bulk user backup.
