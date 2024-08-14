
## Snort Manual.
- snort run as IDS by default
- command arguments
```bash
# don't show banner 
snort -q
# verbose mode
snort -v 
# display the packet data (payload)
snort -d
# Display the link-layer (TCP/IP/UDP/ICMP) headers.
snort -e
# display packet data and link layer
snort -de
# Display the full packet details in HEX
snort -X
# specify interface
snort -i eth0
# logger mode - target log,alert output folder
snort -l ./dir
# ASCII log Format
snort -K ASCII
# read log/pcap file
snort -r snort.log
# specify number of packets to read
snort -r snort.log -n 10
# read list of pcap files
snort --pcap-list="icmp-test.pcap http2.pcap" -n 10 
# read list of pcap files and show files name
snort --pcap-list="icmp-test.pcap http2.pcap" -n 10
## IDS/IPS modes arguments
# specify config file for|or rules file
snort -c snort.config
# test Config file
snort -T -c snort.config
# disable logging packets
snort -N
# run in background
snort -D
## Alert modes full,fast,console,cmg,none
# full: alert everything about the payload 
# fast: alert message, timestamp, source and destination IP, along with port numbers.
# console: Provides fast style alerts on the console screen.
# cmg: CMG style, basic header details with payload in hex and text format.
# none: Disabling alerting. 
snort -A fast
# enable IPS mode require two interfaces
snort -Q --daq afpacket -i eth0:eth1 -A console


```

#### Rule Structure
```css
alert icmp any any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)

alert icmp any any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;reference:CVE-XXXX;)

alert: Action type 
icmp: Protocol Type
any any: Source Ip+Port
<>: direction 
any any: Destintion Ip+Port
Message: msg
Reference: reference
Revesion info: rev

Action: 
	alert: generate an alert and log the packet
	log: log the packet
	drop: block and log the packet
	reject: block, log the packet and terminate the connection
Protocol: ICMP - TCP - UDP

Direction: 
	<>: Bidirectional
	->: Source to destinetion
Options:
	msg: one-liner summerises the event
	Sid: 
	<100: Reserved rules
	100-999,999: rules came with the build
	>=1000000: rules created by user
Reference: additional information explain the purpose of the rule or threat pattern
Rev: help analysts to have revesion information of each rule, it tells how many times the rule has been modified, it is not a back-up
Flags: 
	F - FIN
	S - SYN
	R - RST
	P - PSH
	A - ACK
	U - URG
	PA - PSH-ACK
Dsize:
	dsize:min<>max
	dsize:>100
	dsize:<100
```

#### snort configuration
```bash
# default config file
/etc/snort/rules/local.rules
# default log file
/var/log/snort
# default rules file
/etc/snort/rules/local.rules
```

#### snort Filters
```bash
# multiple ip range
alert icmp [192.168.1.0/24, 10.1.1.0/24] any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)
# execlue ip
alert icmp [192.168.1.0/24, !192.168.1.1] any <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)
# port range
alert icmp any :80,100:150,1024 <> any any  (msg: "ICMP Packet Found"; sid: 100001; rev:1;)
# ASCII Mode detection
alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; sid: 100001; rev:1;)
# HEX Mode detection
alert tcp any any <> any 80  (msg: "GET Request Found"; content:"|47 45 54|"; sid: 100001; rev:1;)
# disable case sensitivity: nocase;
alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; nocase; sid: 100001; rev:1;)
#use the first content option for the initial packet match: fast_pattern, default is the biggest
alert tcp any any <> any 80  (msg: "GET Request Found"; content:"GET"; fast_pattern; content:"www";  sid:100001; rev:1;)
#filter Syn tcp flag
alert tcp any any <> any any (msg: "FLAG TEST"; flags:S;  sid: 100001; rev:1;)
# filter packet payload size between 100-300
alert ip any any <> any any (msg: "SEQ TEST"; dsize:100<>300;  sid: 100001; rev:1;)
# Filtering the source and destination IP addresses for duplication.
alert ip any any <> any any (msg: "SAME-IP TEST";  sameip; sid: 100001; rev:1;)

```
-   snort.conf: _Main configuration file._
-   local.rules: _User-generated rules file._


**Main** Components of Snort
-   **Packet Decoder -** Packet collector component of Snort. It collects and prepares the packets for pre-processing. 
-   **Pre-processors -** A component that arranges and modifies the packets for the detection engine.
-   **Detection Engine -** The primary component that process, dissect and analyse the packets by applying the rules. 
-   Logging and Alerting - Log and alert generation component.
-   Outputs and Plugins - Output integration modules (i.e. alerts to syslog/mysql) and additional plugin (rule management detection plugins) support is done with this component.

**There are three types of rules available for snort**

-   Community Rules - Free ruleset under the GPLv2. Publicly accessible, no need for registration.
-   Registered Rules - Free ruleset (requires registration). This ruleset contains subscriber rules with 30 days delay.
-   Subscriber Rules (Paid) - Paid ruleset (requires subscription). This ruleset is the main ruleset and is updated twice a week (Tuesdays and Thursdays).


#### Snort Configuration File
- Step 1: Set the network variables section
```
HOME_NET : That is where we are protecting.  
 'any' OR '192.168.1.1/24'  
EXTERNAL_NET : This field is the external network, so we need to keep it as 'any' or '!$HOME_NET'.  
	'any' OR '!$HOME_NET'  
RULE_PATH : Hardcoded rule path.  
	/etc/snort/rules  
SO_RULE_PATH : These rules come with registered and subscriber rules.
	$RULE_PATH/so_rules  
PREPROC_RULE_PATH : These rules come with registered and subscriber rules.
	$RULE_PATH/plugin_rules
```
- Step 2: Configure the decoder section
```
config daq: IPS mode selection.
	afpacket
config daq_mode: Activating the inline mode
	inline
config logdir: Hardcoded default log path.
	/var/logs/snort
```
- Step 6: Configure output plugins section
```
This section manages the outputs of the IDS/IPS actions, such as logging and alerting format details. The default action prompts everything in the console application, so configuring this part will help you use the Snort more efficiently.
```
- Step 7: Customise your ruleset section
```
 site specific rules : Hardcoded local and user-generated rules path.
	include $RULE_PATH/local.rules  
include $RULE_PATH/ : Hardcoded default/downloaded rules path.
	include $RULE_PATH/rulename

```

Data Acquisition Modules (DAQ) are specific libraries used for packet I/O, bringing flexibility to process packets. It is possible to select DAQ type and mode for different purposes.
There are six DAQ modules available in Snort:
-   **Pcap:** Default mode, known as Sniffer mode.
-   **Afpacket:** Inline mode, known as IPS mode.
-   **Ipq:** Inline mode on Linux by using Netfilter. It replaces the snort_inline patch.  
-   **Nfq:** Inline mode on Linux.
-   **Ipfw:** Inline on OpenBSD and FreeBSD by using divert sockets, with the pf and ipfw firewalls.  
-   **Dump:** Testing mode of inline and normalisation.

- Snort Cheat sheet !
![[SnortCheatsheetTryHackMe.pdf]]