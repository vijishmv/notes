Normally, I search for tabs in vim using the search (pressing ‘/’) pattern:

\t

Now if you have a text file with a mix of spaced tabs (tabs made using 8 spaces usually) and normal tabs, it doesn’t find both.

The simplest way to visualize the different tabs – (normal tabs and tabs formed with spaces) in vim is to use the vim setting listchars :

:set list

This way you get to see spaced tabs with spaces (no special character) but normal tabs are shown with ^I characters.
