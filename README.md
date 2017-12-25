# Logstash config for System Security Services Daemon (sssd)

This configuration is dived in a few parts.

### Rsyslog configuration

Rsyslog config is very simple, just looks in a file and forward with the syslog config you have already but add a little sssd tag before the message (this is important).

### Logstash configfiles

Logstash configuration is divided in two parts.

Look at the syslog messages and if it contains sssd add a tag for logstash named sssd and also do initial parsing of message.

A log line will look something like this.

sshd TIMESTAMP (sssd timestamp) [logging facility in sssd] some info and message

The initial parsing will put the logging facility from ssd into the field sssd_type

Then this message will be picked up by the second part of the parsing, where an if else-block will look at sssd_type and then send that to the parsing of those lines.

the if-else block is so the most common lines will be parsed first, and less common lines will go further down the line.

### Grok patterns

Grok patterns are one per unique line in the logfiles I have seen so far. And then there is a compound pattern for each sssd_type in the bottom of the patterns file. To de clutter the logstash config.

