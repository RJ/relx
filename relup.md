## My relup fork notes

step 1:
copy/paste/edit the relup generating code from otp, which has not really
been updated in forever, and make some changes.

starting with:

if no .appup exists, optionally assume {restart_application, ...}

this makes upgrading large releases, where you want to update 3rd party
deps, much easier. mostly you dont care about state in 3rd party deps so
restarting them is acceptable.


### other thoughts

systools.hrl defines #release and #application, which are passed thru to
systools_rc to translate from high level appup instrs to low level relup
ones

these records are duplicating state that relx already has. if we
reimplement systool_rc:translate blah to use relx frmat we can do away
with it

first step: make our systools_relup replacement use relx's data
structures, but translate them to the systools.hrl ones before sending
for translation to relup.

then consider replacing the translating code


all external calls made from a module:

rp(lists:usort([Target || {_Source, Target} <- element(2,xref:q(s, "(XC)
| irccloud_user"))])).

get list of mods for appup then check which of them call into other mods
from the appup list and add to the Deps in the instructions, eg assuming
irccloud_user and irc_db are being updated in this appup:

{load_module, irccloud_user, [irc_db]}.
