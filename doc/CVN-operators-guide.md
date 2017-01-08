# CVN operators guide

This document describes the requirements for setting up and operating a CVN (certified validation node) for the FairCoin network.

For online discussion and support please join the FairCoin CVN Operators telegram group here: https://telegram.me/joinchat/Bumu8gbZmAhnXoxCBr3WHg

## 1 About CVNs
The aim of a CVN is to secure the network by validating all the transactions that had been sent to the network and put them into a transaction block chain. Blocks are created every 3 minutes (180 sec.). Transactions are confirmed after they have been added to a block. A CVN is a standard FairCoin core client configured to use a smart card for continuously signing certain pieces of data of the FairCoin network with certification data issued by FairCoop. Every node will be assigned a unique id called the CVN ID.

## 2 Technical information for running a CVN

### 2.1 System requirements
A secure and stable operating system is crucial for reliably running a CVN. Therefore all requirements shown in the following list need to be met by the CVN operator.

1. A Linux operating system with a current distribution is required to run a CVN. We recommend using Ubuntu or Debian. All available system updates must be applied on a regular basis.
2. The system must provide at least 1GB of free hard drive space, the more the better.
3. The FairCoin wallet software needs be updated as official releases are made available by the development team.
4. The system must be connected to the Internet all the time (24/7) and the TCP port 40404 must be reachable by all remote nodes from the Internet. Avoid using a wireless connection if possible as this would result in additional network latency and potential unstable internet connection.
5. The system must use a public NTP server to synchronize its system time to, e.g. pool.ntp.org to ensure that the system time is always correct. NTP means Networking Time Protocol and is intended to synchronize all participating computers to within a few milliseconds of Coordinated Universal Time (UTC).
6. The hardware must provide a USB 2.0 port which takes up the USB card reader. Please avoid using a USB-hub for the reader.
7. Although not absolutely necessary, a UPS (uninterruptible power supply) would be appreciated. Note that all network related equipment should also be hooked up to the UPS to keep the network connection up in the event of a power outage.

### 2.2 Preparing your system
To build the FairCoin wallet software you need to install some software packages on your system. Do this by executing the following commands in a terminal:

```sudo apt-get update```  
```sudo apt-get install -y build-essential libtool autotools-dev autoconf pkg-config libssl-dev libboost-all-dev git libdb5.3-dev libdb5.3++-dev libqt4-core libqt4-gui libqt4-dev libqrencode-dev libprotobuf-dev protobuf-compiler libevent-dev libpcsclite-dev pcscd```

Please note that the package names and/or versions can differ depending on your operatin system. E. g. your system might provide a different version of libdb. If it doesn't find version 5.3 try version 5.1.
After package installation has completed successfully proceed by following the instructions provided here:
https://github.com/faircoin/faircoin2/blob/faircoin2/doc/build-CVN.md

Make sure your card reader is connected to your system and is working properly. You can test it by execute the following commands:
```
export PATH=/opt/faircoin/OpenSC/target/bin/:$PATH
pkcs11-tool -T
```

This will display a list of all connected readers with inserted cards. It should list something like this:
```
Available slots:
Slot 0 (0x0): ACS ACR 38U-CCID 02 00
  token label        : SmartCard-HSM (UserPIN)
  token manufacturer : www.CardContact.de
  token model        : PKCS#15 emulated
  token flags        : rng, login required, PIN initialized, token initialized
  hardware version   : 24.13
  firmware version   : 1.2
  serial num         : DECM0104412
```

### 2.3 The first start of your node
Create a configuration file in the data directory of FairCoin and copy-and-paste the following command sequence in your terminal:

```
mkdir ~/.faircoin2
cat > ~/.faircoin2/faircoin2.conf <<EOF
#FairCoin CVN configuration file
logtimestamps=1
logips=1
gen=1
cvn=card
cvnpin=123456
EOF
```

Now start the FairCoin wallet with:
```
/opt/faircoin/faircoin2/src/qt/faircoin-qt -gen=0
```

It will then start to download the block chain, which will take some time, a matter of minutes or maybe hours. The progress bar at the lower left corner of the wallet software displays the download status.
When it is finished, quit the wallet software.

### 2.4 Normal startup of a CVN
You start your CVN with the following command. In terminal or by pressing Alt+F2 on your desktop:
```
/opt/faircoin/faircoin2/src/qt/faircoin-qt
```

Now your CVN is up and running and ready to be activated by the chain administrators.

## 3. Fees and economic model for sustainably running a CVN
In FairCoin transaction fees are designed to be the long term key factor for covering the costs of running a CVN and to prevent transaction SPAM. Because no new coins will be created, fees are the only reward for CVN operators.

We expect that in the short term, this will not be enough to cover electricity costs, so running a CVN can be understood as an act of solidarity. On the other hand, as the economic activity of FairCoin grows, the transaction activity in the blockchain will grow too, as will the income made by running a CVN, so at some point this should be enough to cover the costs of operation.

When the profit exceeds the investment, the open FairCoop assembly could decide to reduce the fees or to pool a part of the incomes for FairCoin development or infrastructure costs. Also, in the future we may agree on using other methodologies for avoiding spam risks and to cover the CVN expenses.

Fees are mandatory and dynamically adjustable. The effective fee per Kb is decided in the FairCoop assembly. On June 16th 2016, in a corresponding assembly (https://fair.coop/docs/minutes-of-the-10th-faircoop-open-assembly/) it has been decided to start with a fee of 0.1 FAIR per 1000 bytes. This is an open process of discussion where every FairCoin user can be involved.

## 4. Organizational process for running a CVN

### 4.1  Socio-political criteria for running a CVN
The person responsible for certification should follow KYCVN criteria (Know your CVN). This means that online or physically, some questions should be asked to get to know the people or groups applying for certification and their real motivations.

Online information like a website, profiles in social networks like fair.coop, bitcointalk, twitter, facebook, etc. could help to confirm that information. If possible, applicants should visit a FairCoop local node nearby. This helps to get to know them better and thus ensure that they have the right intentions.
Positive criteria for CVN acceptance could be:

- Participation in the FairCoop ecosystem.
- Confirmed experience in other activities that share the FairCoop principles in general.
- Agree to values such as fairness, cooperation, circumspection, balance, patience, sharing.
- Active and visible participation in other projects connected to FairCoin or FairCoop.
- Availability within weeks, in emergency case within a day.
- Must have read latest version of FairCoin2 white paper (it is okay, if a CVN operator does not understand all technical details of the FairCoin blockchain mechanism at the beginning, but an operator should be willing to learn and share his/her experience).

### 4.2 Procedure
The certification process will be developed in a decentralized way by the various FairCoop groups.

1. One global procedure will be led by the CVN working group of the FairCoop assembly which will receive applications, support the creation of CVNs worldwide, and will present the applicants to the general FairCoop assembly. In general FairCoop assemblies will finally be decided about the approval of a CVN. Alternatively, specific assemblies could be convened for approving new CVNs to the CVN working group if there are any urgent conditions.

2. There will be regional procedures in areas where a FairCoop local node is available. FairCoop local nodes will receive the CVN applications in their area and will confirm that the goals of the applicants are aligned with proof of cooperation needs. The FairCoop local nodes should be able to make decisions about this in their open assemblies.

### 4.3 Spread of CVN certification
At least 60% of the certifications should be processed by FairCoop local nodes, and at most 40% by the global procedure. In this way, not even an attack on the global FairCoop structures would be enough to attack 50% of the network. As soon as more and more FairCoop local nodes become active, the dependence on the global procedure can be reduced. In order to have a healthy distributed procedure, a unique FairCoop local node, can't certify more than 10% of the CVN network, and the maximum number of CVNs that can be set up will be determined in the global FairCoop assemblies.

### 4.4 Transparency
The results of the certification processes will be published. So everybody can understand why an application was approved or denied.
The global CVN working group can refuse approvals for security reasons.

### 4.5 Removal of CVNs
In the event malicious behaviour of a CVN is detected, it is removed instantly by the chain administrators. Re-application of such a person to become a CVN operator is not possible in the future.
CVN operators can request removal if they are no longer willing or able to run a CVN.

The removal of a CVN will be performed by the chain administrators.

### 4.6 How to join
If you are interested in setting up a CVN, please fill out the form here https://ocp.freedomcoop.eu/joinaproject/faircoin2-cvn-operators/. 

We will be able to begin when at least 15 CVN are ready. After that, the open process will continue to welcome more CVNs to secure the network further.

More information and discussion is avaiable here: https://fair.coop/groups/faircoop-community/faircoin/faircoin2/ or contact us  at cvn@fair-coin.org .
