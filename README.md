# Decoy-sploit
Bunch of honey related items that spoof powersploit. The reason for chosing powersploit is that it is a good example of a lot of TTPs that attackers/red teamers use that are also in other tools- this means that we should be able to kill many birds with one stone.

Starting small, and adding more as I add in interesting methods. Wider concepts can be found on my resource repo : https://github.com/s0lari/Hornets-Nest

**Disclaimer - test this in your test environment, if it breaks anything in your prod, then that is your responsibility.**

Generally anything around https://canarytokens.org is a winner - add the various terms the system is looking for in the file (pdf, word etc)


pass

sensitive

admin

login

secret

unattend1.xml

creds

credential

.config

.vmdk

Log, alert at SIEM - you'll also get an email via canarytokens itself - creating a custom, more realistic file that an attacker may download to their own system and open would be the best result as this will trigger the machine that they're using - this may simply be a bastion host though, so it isn't perfect. Generally had more success with .pdf files as the canarytoken seems to trigger on the dns lookup, which Adobe appears to do even if you tell it to not access that request to external content (like some kind of weird prefetch).


## Get-GPPassword.ps1

Three pre-made decoys in this repo:

1) Blank scrolling - this Group.xml file is 5000 lines of blank followed by a single entry that can be adjusted to your needs if you use my Cyber-Chef recipes here https://github.com/s0lari/Hornets-Nest#cyberchef-recipes - just reverse it, make your changes, then re-encrypt. Reason for this is if you put the Groups.xml file in the root of Policies, it will be processed last, which then means the others passwords will be pushed up off the powershell view without additional powershell listing commands.
2) Doppleganger output - this will create a list of outputs that look mostly like powersploit's output - you can add/remove users and PW in these entries.
3) Rick Roll - Can't have honey-related stuff without Rick Rolling :) 

Hopefully add more to this based on the rest of the functions of PowerSploit.

You can place these in a similar location in your domain. 

"\\\DEMO.LAB\SYSVOL\demo.lab\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml"

If you're forced into a situation where you cannot get rid of your GPPassword file, you could always generate a lot of them with random similar looking permutations of the password that you have in your environment, then trigger off these if possible to your SOC. Perhaps even have them genuinely authenticate into a honeypot/honey network so they spend some time there instead of in your prod environment.

John the ripper can be used to generate passwords based off a single entry:

1) Create a single txt file containing your password
2) type : john --wordlist=<your wordlist.txt> -rules:o2 -stdout > mangled-password.txt
3) You can create a smaller list by using : john --wordlist=<your worldlist.txt> -rules:Wordlist -stdout > managled-password.txt
4) ???
5) Profit!



## Recon module - Find-interestingFile / Invoke-FileFinder

Load a command prompt and run the following script - it will output a ~1gb file containing terms that this script finds by default - feel free to add some more - makes it fun for if they are exporting to a csv file - the more terms you add the larger the csv results. Could also just grab a password dump and leave that somewhere - but that may be helping them a bit too much ;) 

echo pass >> pass.txt

echo sensitive >> pass.txt

echo admin >> pass.txt

echo login >> pass.txt

echo secret >> pass.txt

echo unattend1.xml >> pass.txt

echo creds >> pass.txt

echo credential >> pass.txt

echo .config >> pass.txt

echo .vmdk >> pass.txt

echo password >> pass.txt

for /L %i in (1,1,50) do type pass.txt >> pass.txt

If you change the '50' to a larger number it will result in much larger file sizes.

Place this file on a generally accessible share - also set it up for file auditing and logging to SIEM. Whitelist any backup process service accounts. Job done!

## Invoke-Kerberoast

To detect this, I'd suggest this : https://github.com/s0lari/Hornets-Nest/blob/master/README.md#kerberoasting-honey-spn

## Invoke-ReverseDnsLookup

This will look for PTR records associated with IP ranges. Configure your DNS server to log DNS, forward these logs to your SIEM, create a decoy PTR record for various ranges that you use (use a decent name), but ensure the associated IP isn't genuine. Log whenever this PTR record is queried from your DNS server.

## Find-DomainShare / Invoke-ShareFinder

Pretty simple - create a share on a random system that is domain joined, and log all attempts to access the share, send logs to SIEM, alert, winning! Ensure that this share is not added to users generally and would specifically take someone going to the machine directly to view the share resources.
