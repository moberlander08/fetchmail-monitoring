This is a script that I wrote quick to help me monitor some IMAP mailboxes via a nagios check.  

You will need to use the fetchmail perl script for this script to work.
https://github.com/rtcommunity/fetchmail_testing

Once you have the code from the fetchmail_testing repo in place you will need to generate a fetchmail file for the script to check against.

1: Dump fetchmailrc to a Python data structure using %>fetchmail --configdump -f fetchmailrc > fetchmailrc.py
2: Convert Python data structure to JSON. %>python fetchmail.py > fetchmailrc.py.json
3: prove -v t/test.t :: fetchmailrc.py.json

Right now the fetchmail_testing code is set at a hard limit of 5 messages for an alert, make sure to edit this to match your needs.

Once you have the configuration add the following command to the nagios commands file:

	define command{
        	command_name    check_rtmailboxes
	        command_line    $USER1$/check_fetchmail $ARG1$
	}


Then add the nagios.cfg file the the nagios resourcese.d folder, make sure to edit the check to match your outputed fetchmail configuration files.

From there simply update the script to include a destination email address, such as a support dlist.

Note: I have the script setup to output to the /usr/lib64/nagios/email-monitoring/ directory so you this directory needs to be created and owned by nagios:

	mkdir /usr/lib64/nagios/email-monitoring
	chown -R nagios:nagios /usr/lib64/nagios/email-monitoring

Feel free to modify this script as you see fit.
