# Decoy-sploit
Bunch of honey related items that spoof powersploit.

Starting small, and adding more as I add in interesting methods.

Three to start based on the Get-GPPassword.ps1 file:

1) Blank scrolling - this Group.xml file is 5000 lines of blank followed by a single entry that can be adjusted to your needs if you use my Cyber-Chef recipes here https://github.com/s0lari/Hornets-Nest#cyberchef-recipes - just reverse it, make your changes, then re-encrypt
2) Doppleganger output - this will create a list of outputs that look mostly like powersploit's output - you can add/remove users and PW in these entries.
3) Rick Roll - Can't have honey-related stuff without Rick Rolling :) 

Hopefully add more to this based on the rest of the functions of PowerSploit.

You can place these in a similar location in your domain. 

\\DEMO.LAB\SYSVOL\demo.lab\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml


**Disclaimer - test this in your test environment, if it breaks anything in your prod, then that is your responsibility.**
