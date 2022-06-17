% bub(1) bub 1.0.1
% Colin van Loo
% June 2022

# NAME

bub - Bulk backup users' home directories.

# SYNOPSIS

**bub** [*ARGUMENTS*] [*INPUTFILE*]

# DESCRIPTION

**bub**, short for "Bulk User Backup" creates tar archives from users' home
directories. If no input file is provided, **bub** will search for a file set
in its configuration.

If the **-h**|**--help** or **--version** flags are set, **bub** will exit
immediately after printing the help or version information.

This script must be run as root.

# OPTIONS

**-h**, **--help**
: Display help information and exit immediately.

**--version**
: Print version and exit immediately.

**--config *FILE***
: Read cofig from FILE

**-g *GROUP***, **--group *GROUP***
: Backup users part of *GROUP*(s). This overwrites the value from the
configuration file.

**-d *DIRECTORY***, **--directory *DIRECTORY***
: Save backup to *DIRECTORY*.

**-e *PATTERN***, **--exclude *PATTERN***
: Exclude all files matching one of the *PATTERN*(s)

**-p *PATTERN***, **--pattern *PATTERN***
: Pattern to generate output file name from. For example `$(date
'+%Y.%m.%d-%H:%M:%S')-backup`.

**-k *NUMBER***, **--keep *NUMBER***
: Maximum number of backups to keep. Oldest backups get deleted.

**-v [*(D|I|W|E)*]**, **--verbose [*(D|I|W|E)*]**
: Verbosity level. If no level provided, the *D* is used.

# EXAMPLES

**bub --help**
: Display help and exit.

**bub --version**
: Display version and exit.

**bub -v**
: Be verbose.

**bub -e \*bash\*,\*_hidden**
: Exclude all files containing *bash* or *\_hidden* in their file name.

# EXIT VALUES

**0**
: Success

**1**
: Not run as root

**3**
: Invalid configuration file

**255**
: Other error

# BUGS

Lol.
