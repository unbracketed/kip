% KIP(1)
% Graham King
% 26 OCT 2012

# NAME

kip - Keeps Internet Passwords. Command line script to keep usernames and passwords in gnupg encrypted text files.

# SYNPOSIS

kip <filename|filepart>
kip <command> [<filename|filepart>] [option...]

# DESCRIPTION

Kip can be used to securely store credentials on your system. Each set of
credentials is stored in a file and encrypted and signed with your GPG key.
Kip is just a convenience tool; you can use and manage the password files
kip creates without needing kip.

# INSTALL

## DEPENDENCIES

Run the following to make sure dependencies are installed.

Ubuntu: `apt-get install gnupg xclip python3`
OSX: `brew install gnupg python3`

**GnuPG**

You'll need to have a GnuPG key pair. Kip uses it to encrypt and decrypt
your password files.

[GnuPG HOWTO](https://help.ubuntu.com/community/GnuPrivacyGuardHowto).

**Sytem Clipboard - xclip / pbcoby**

For Ubuntu use `xclip`. For OSX use pbcopy, which is installed by default.

**Python 3**

Kip uses Python 3.

**Note for Tmux users on OSX**

On OSX, if you use kip from within a tmux session, passwords won't get
copied to the system clipboard. You can install a tmux extension and configure
tmux to work correctly. See [Tmux Copy & Paste on OS X: A Better Future](http://robots.thoughtbot.com/post/55885045171/tmux-copy-paste-on-os-x-a-better-future) for more information.

## INSTALL KIP AS PYTHON PACKAGE

Latest release (you may need to run this as root):

    pip install kip

Latest dev:

 1. Clone the repo: `git clone https://github.com/grahamking/kip.git`
 2. Install: `cd kip && sudo python3 setup.py install`

## INSTALL KIP AS OS PACKAGE

**Ubuntu**: [PPA with 'precise' package](https://launchpad.net/~graham-king/+archive/ppa)

**Arch Linux**: [kip package for Arch](https://aur.archlinux.org/packages.php?ID=62555).
Thanks [Pezz](https://github.com/pezz)!

**OSX**: No packages available at this time.


# USING KIP FOR STORING PASSWORDS


If you're creating an account somewhere, or would like to have kip generate
a new secure password for an account:

    kip add example.com

Kip will prompt you for a username, or you can specify on the command line:

    kip add example.com --username fletch

If you already have a password, or would like to create your own:

    kip add example.com --username dr_rosen --prompt

After adding an account to kip, your password will be available on the system
clipboard. You can retrieve your password at a later using the `get` command:

    # The `get` command is the default for kip
    kip example.com
    # You can also specify `get` explicitly:
    kip get example.com
    # You can reference password files with partial names
    kip example

Get a list of accounts kip is managing:

    kip list


# COMMANDS

## add

`kip add example.com --usename username`

What it does:

 1. Generates a random password
 2. Writes username and password to text file `~/.kip/passwords/example.com`
 3. Encrypts and signs it by running `gpg --encrypt --sign --armor`
 4. Copies the new password to your clipboard

Add optional notes: `kip add example.com --username username --notes "My notes"`.
You can ask to be pompted for the password, instead of using a random one: `kip add example.com --username username --prompt`

## get

`kip example.com`

What it does:

 1. Looks for `~/.kip/passwords/*example.com*`, decrypts it by running `gpg --decrypt`
 2. Prints your username in bold, and any notes your stored.
 3. Copies your password to the clipboard

## list

`kip list "*.org"`

List contents of your password directory. [filepart] argument is a glob to filter the directory list. You can use ls too!

## edit

`kip edit example.com --username newuser`

Change the username inside a password file.  [filepart] is the file to edit, and --username sets a new username.

## del

`kip del example.com`

Delete a password file. [filepart] is the file to delete. You can use rm too!

## import\_from\_chrome

Import passwords that Chrome stored in Gnome Keyring. This requires gnomekeyring (python lib) and python2.


# CONFIGURATION

If you want to use different commands to encrypt / decrypt your files, want longer passwords, etc, you can.  Copy `kip.conf` from the repo to `~/.kip/kip.conf`, and customise it. It's an INI file, using = or : as the delimiter. Make sure the `home` path does not end with a slash.

# NOTES

[GnuPG](http://www.gnupg.org/) is secure, open, multi-platform, and will probably be around forever. Can you say the same thing about the way you store your passwords currently?

I was using the excellent [Keepass](http://en.wikipedia.org/wiki/KeePass) when I got concerned about it no longer being developed or supported. How would I get my passwords out? So I wrote this very simple wrapper for gnupg.

If you live in the command line, I think you will find **kip** makes your life a little bit better.

# FILES

There's 0 magic involved. Your accounts details are in text files, in your home directory. Each one is encrypted with your public key and signed with your private key. You can ditch **kip** at any time.

Browse your files: `ls ~/.kip/passwords/`

Display contents manually: `gpg -d ~/.kip/passwords/facebook`
