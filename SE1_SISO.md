---
permalink: /SE1_SISO/
title:  "SE1: Multi-Node LTE SISO"
---

# 1) Experiment Overview
The minimal representative working environment for this experiment is with LW1 as the AFRN and any APRN. At the end of the experiment, the user can view logs and test results. The main goal of this experiment is to provide a complete end-to-end LTE network, using srsUE with srsENB and srsEPC. This experiment can be run in EMULATION or TESTBED modes.

```diff
+ Can be mentioned that the default run mode is EMULATION
+ Info about the type of vehicle to be set for each node can be added
+ the difference between testing the experiment in EMULATION & TESTBED modes could be clarified

+ If the user wants to run the experiment in TESTBED mode, they have to put in the proper request ..
+ .. when creating the experiment.
```

## Description:
This mode sets up the evolved packet core (EPC) and eNodeB (eNB) on one machine and one or more user equipment (UEs), each on separate machines. A connection will be established between each UE and the eNB. If the default configuration settings are used, the radios will operate within the 3.3 - 3.55 GHz spectrum (Band 22) at 5 MHz bandwidth (25 Resource Blocks) using frequency division duplexing (FDD). 

This experiment optionally supports ping and iPerf as traffic scripts. 

### Software Versions:
Ubuntu version 22.04

srsLTE version 23.04

UHD version 4.3


## Most Common Configuration Parameters:
<ul>
<li>EARFCN (center frequency): this sets the center frequency of the downlink transmission for 3GPP supported bands. The uplink center frequency is automatically set, and it depends on whether the band is TDD (same uplink and downlink frequency) or FDD (uplink and downlink offset by the channel spacing). </li>

<li>LTE system bandwidth, configured as resource blocks (RBs): supported values are 6 RBs for 1.4 MHz channel bandwidth, 15 RBs for 3 MHz, 25 RBs for 5 MHz, 50 RBs for 10 MHz, 75 RBs for 15 MHz and 100 RBs for 20 MHz. </li>

<li>USRP Tx Gain (sets the gain of variable PA in the USRP in the range of 0 to 89 for B series, 0 to 30 for X310): default is 0 dBFS which has been determined for the external Tx RF front end. </li>

<li>USRP Rx Gain (sets the gain of variable PA in the USRP in the range of 0 to 76 for B series, 0 to 30 for X310): default is 0 dBFS which has been determined for the external Rx RF front end and the Tx/Rx isolation. </li>
</ul>

## 2) Performing the Experiment

### LTE Base Station (eNodeB or eNB) Configuration:
An Experimenter can run the eNB on a fixed or a portable node. In what follows it is assumed that the eNB is assigned to a fixed node. Login to the E-VM corresponding to the fixed node. Navigate to the folder containing all the Radio scripts:



```bash
$ cd /root/Profiles/ProfileScripts/Radio
```

Copy the srsRAN SISO script as startRadio.sh

```bash
$ cp Samples/startSRSRAN-SISO-EPCandENB.sh startRadio.sh 
```

Use an editor to uncomment the line  `/Radio/startRadio.sh` in `/root/startexperiment.sh`:
```diff
+ $ nano /root/startexperiment.sh

# People might skip non-block-styled code and go directly to code blocks.
```

and run the following command:

```bash
$ /root/startexperiment.sh
```

Alternatively, the experiment can be started from the OEO Console.


## User Equipment (UE) Configuration:
The UE can run on a portable or a fixed node. In what follows it is assumed that the UE is assigned to one or more portable nodes. Login to the E-VM corresponding to the portable node(s). Navigate to the folder containing all the Radio scripts:

```bash
$ cd /root/Profiles/ProfileScripts/Radio
```

Copy the srsRAN SISO script as startRadio.sh

```bash
$ cp Samples/startSRSRAN-SISO-UE.sh startRadio.sh 
```

Use an editor to uncomment the line  `/Radio/startRadio.sh` in `/root/startexperiment.sh`:
```diff
+ $ nano /root/startexperiment.sh

# People might skip non-block-styled code and go directly to code blocks.
```
and run the following command:

```bash
$ /root/startexperiment.sh
```

Alternatively, the experiment can be started from the OEO Console. Repeat the UE setup for each of the nodes intended as UEs (if more than one UE is present in the Experiment).

```diff
+ Running startexperiment.sh on both nodes yields no feedback, which could confuse users ..
+ ..about whether the script did anything at all.
```


## 3) Modifying the Experiment Configuration Parameters:

```diff
+ It should be mentioned at the very start that modifying the parameters is entirely optional... 
+ ...Even though it is stated in each node section as 'which can be,' some users might overlook/misinterpret it.

# For clarity's sake, the term "optional" could be added within parantheses in the heading.
```

Core Network and Base Station:
The shell script `/root/Profiles/ProfileScripts/Samples/startSRSRAN-SISO-EPCandENB.sh` calls two scripts `/root/Profiles/ProfileScripts/Radio/Helpers/startEPC.sh` and `/root/Profiles/ProfileScripts/Radio/Helpers/startENB.sh` which can be edited by the experimenter to modify the parameters of the core network and base station.

```diff
- /root/Profiles/ProfileScripts/Samples/startSRSRAN-SISO-EPCandENB.sh
+ /root/Profiles/ProfileScripts/Radio/Samples/startSRSRAN-SISO-EPCandENB.sh

# Radio/ Vehicle/ Traffic, but we are concerned with 'Radio' in this specific experiment.
```


* tx_gain (in dB) controls the gain of the USRP transmitter. Reducing the value will reduce the strength of the radio signal (and correspondingly the transmission range). Increasing it may lead to distortions (see results section below). 

* rx_gain (in dB) controls the gain of the USRP receiver. Similar to the tx_gain, significant changes can lead to reduced range or distortions on the received signal. 

* dl_freq and ul_freq are the downlink and uplink frequencies respectively. Changes at the eNB frequencies must be coordinated with changes at the UE frequencies (or they will never find each other). 

* n_prb controls the number of resource blocks 


## UE:

The shell script `/root/Profiles/ProfileScripts/Samples/startSRSRAN-SISO-UE.sh` calls the script `/root/Profiles/ProfileScripts/Radio/Helpers/startUE.sh` which can be edited by the experimenter to modify the parameters of the UE:)
```diff
- /root/Profiles/ProfileScripts/Samples/startSRSRAN-SISO-UE.sh
+ /root/Profiles/ProfileScripts/Radio/Samples/startSRSRAN-SISO-UE.sh

# Radio/ Vehicle/ Traffic, but we are concerned with 'Radio' in this specific experiment.
```

* tx_gain (in dB) controls the gain of the USRP transmitter. Reducing the value will reduce the strength of the radio signal (and correspondingly the transmission range). Increasing it may lead to distortions (see results section below). 

* rx_gain (in dB) controls the gain of the USRP receiver. Similar to the tx_gain, significant changes can lead to reduced range or distortions on the received signal.

* dl_freq and ul_freq are the downlink and uplink frequencies respectively. Changes at the UE frequencies must be coordinated with changes at the eNB frequencies (or they will never find each other).

* imsi is setup to values that correspond to the eNB setup in the `user_db.csv`. By default, the imsi value setup the script `/root/Profiles/ProfileScripts/Radio/Helpers/startUE.sh`, which defaults to `1010123456700 + $node_num`, meaning 10101234567xy where xy is the node number (e.g., xy=03 for node 3). This imsi number is then used by the eNB to assign an IP address to the UE (upon connecting to the eNB), by looking up the imsi in the file `user_db.csv` as described in section 4.1.1) SRSRan Experiments. Note that changes in the imsi values without a corresponding change in `user_db.csv` will likely lead to random IP assignments for the UEs.

## IP Address Assignment: 
The bash script `startUE.sh` assigns unique international mobile subscriber identities (IMSIs) to UEs based on their node numbers. The mapping between the IMSI and the IP address is defined in a file `/root/.config/srsran/user_db.csv`  on the EPC side. Based on this mapping the network elements are assigned the following IP addresses: 

```bash
eNB is assigned 172.16.0.1

UE at node number 1 is assigned 172.16.0.100

UE at node number 2 is assigned 172.16.0.2

UE at node number 3 is assigned 172.16.0.3

...

UE at node number 99 is assigned 172.16.0.99
```

For advanced users, the IP address of the UEs can be controlled as documented in section 4.1.1) SRSRan Experiments.

## Supported Traffic:
### PING

Description: Ping measures the round trip time it takes for a small data packet to be transmitted from a source node to another node and back. The ping time is measured in milliseconds (ms). 

The IP assignment process is described in the IP Address Assignment subsection (above).  

The procedure of generating ping traffic is given here. No special setup is required at the target node.

### IPERF

Description: iPerf has client and server functionality, and can create data streams to measure the throughput and packet loss between two nodes.

The IP assignment process is described in the IP Address Assignment subsection (above).  

The procedure of setting up iPerf traffic is described here. 



## 4) Sample Results:
Both in emulation and testbed, the results are automatically saved in files `/root/Results/` of each node that has the running radios or traffic scripts. Furthermore, in emulation mode the results can be seen live by switching the corresponding screens. 


### 4.A) Ping Results:
The ping results are saved in /root/Results/<Date>_<time>_traffic_log.txt and can be seen live in emulation mode by switching to the traffic screen.  


Example results with a eNB pinging UE2 are shown in Figure 1.
<img src = "/aerpaw_feedback/images/SE1_SISO/1.png">
<br>
Fig 1: Ping Results

## 4.B) Sample eNodeB Results:
The logging/ trace data for the eNodeB, shown in Figures 3 and 4, and for the EPC, shown in Figure 5, will be printed to the /root/Results folder.

The trace shows typical radio parameters, specific to the protocol. In this case, the protocol is LTE, and the following uplink and downlink specific parameters are provided by srsran:

* Radio Network Temporary Identity (RNTI) is related to the network ID.

* Channel quality indicator (CQI) is a value between 1 and 15 indicating the quality of the channel.

* Rank indicator (RI) relates to the rank used for multi-antenna/ MIMO configurations.

* Modulation and coding scheme (MCS) refers to adaptive modulation and coding that LTE supports to adjust the bit loading to the given channel conditions.

* OK/NOK are positive and negative acknowledgements.

* BLER (%) is the percentage of dropped blocks. It should be below 10% for the system to function according to the specifications.

* PUSCH and PUCCH refer to the physical uplink shared and control channels and here measure their SNRs.

* PHR is the power headroom.

* The bitrate (brate) is the throughput in bits per second (k for kilo, M for Mega).
<br>
<img src= "/aerpaw_feedback/images/SE1_SISO/2.png">
Fig. 3. srsENB.log with trace data for 2 UEs
<br>
<img src="/aerpaw_feedback/images/SE1_SISO/3.png">
Fig. 4. Logging at the eNodeB (srsENB.log)
<br>
<img src="/aerpaw_feedback/images/SE1_SISO/4.png">
Fig. 5. Logging at the EPC (srsEPC.log)
