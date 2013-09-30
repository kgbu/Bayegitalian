Bayegitalian
============

cloud-native event (syslog, SNMPtrap etc.) analysing system

client side

process:
	push to SQS(client_signature, message_body)
		or
	just emit syslog (TCP/UDP 514)

syslog-ng_pusher:
	push to SQS({Hostname, IPaddr, ProcessName, Level }, Body ) 

mail_pusher:
	POP/IMAP SNMP trap emails
	push to SQS({Hostname, IPaddr}, MIB-Body)

analyzer worker process:
    receive from SQS
	{ {Hostname, IPaddr, ProcessName, Level }, Body } 
		-> analyzeSyslog(Body);
	{ {, Body } 
		-> 
	{ UnknownTrapSignature, Body } 
		-> log(UNSUPPORTED, {UnknownTrapSignature, Body})
    end.


analyze (Bayesian Filtering)
	Normalization required? or not?

alert maker
	make query (Latestness, criteria)
	push hit entries to (AWS) SNS

make response (by Human) to educate Bayesian Filter

