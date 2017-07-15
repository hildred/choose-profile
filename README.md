# Choose Profile

This is a set of udebs to add a preseedable menu entry to Debian installer. At
this stage it is not complete or functional and would only be of interest to
collaborators. For details contact me directly or follow
debian-boot@lists.debian.org.

I have always held that one issue with UNIX backup programs is that restores
require UNIX which works fine for a great number of use cases, but is difficult
in the case of restoring from a catastrophic failure and testing for restoring
after catastrophic failures (after all if you don't test your backups how do
you know they work?), so I figured that the best way to deal with this is to
have your backup set include an installer or bootable restore, and my favorite
installer is of course d-i.

Now I have a chance to build a piece toward that vision of having d-i be a
backup restore utility, and I am getting paid to do it. The idea is that the
backup utility would create a pressed file to be used with d-i that would as
part of the install (or possibly in a restore mode like rescue mode) restore
from backups. I'm not yet doing that part because in this case the backup only
needs done once as the machines do not change much (and then only after much
testing). What I need to do is automate the restore and in doing so I plan to
add two additional menu entries to d-i when properly preseeded. I am
tentatively calling them 'choose profile' and 'install data'.

################################################################################
In a typical install without a preseed file neither option will show in the
menu.

'Choose profile' will be executed as early as possible to allow choice of
preseed files to allow choosing which preseed profile to use. I have been
hemming and hawing about where to put it for maximum versatility as it would be
nice to be able to use it to choose the language after logging in over ssh
using the network console (I want a pet unicorn too). It will ask one question
and have two hooks based on the answer. One hook will load an additional
preseed file, the other will execute a script.

'Install data' will not ask any questions and will be executed as late as
possible (right before finish install) It will have three hooks that will be
preseeded by the additional preseed file loaded by choose profile. The first is
a set of archives to be installed (progress bar is useful here) and the other
two are both script hooks: prepare and finish, executed before and after the
install of archives.

There are other uses cases such as deploying so called cattle servers. The way
this will be used is that as soon as d-i asks which profile to use, the user
can choose between prepared configurations or backups.

Some thoughts on translation: Except for an optional default of 'none - normal
install' the choices do not need translation as they will be site specific,
localized at the creation of the preseed file, and would not make be useful
outside the local site, for example backups would typically named on the
machine and date the of the backup, or a web server based on the site it would
display or role it was to play (reverse proxy, static content, dynamic content,
backend, etc.) and in my case it is German and no one knows what it means any
way so it doesn't matter. On the other hand the question and titles would be
subject to translation (if the language has already been preseeded or passed in
the environment by ssh) At the moment the two strings I am thinking about using
in addition to the aforementioned none are 'Choose profile' for the question
and 'Local data' for the progress bar. Suggestions for improvement are welcomed.

