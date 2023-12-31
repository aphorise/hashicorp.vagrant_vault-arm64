# LAST RUN:

```
reset && time vagrant up --provider virtualbox ;
```

Version & brief details of last run near to the time of last commit.

```
real	1m21.527s

2023-12-11 @ 12:38 ;
Darwin 23.1.0 Darwin Kernel Version 23.1.0: Mon Oct  9 21:32:11 PDT 2023; root:xnu-10002.41.9~7/RELEASE_ARM64_T6030 Darwin macOS ProductVersion:		14.1.1 ;
CPU: 12-core Apple M3 Pro (-MCP-) speed: 0 MHz OS: 23.1.0 arm64 Up: 0h 38m;
VMware Fusion Information:
VMware Fusion 13.5.0 build-22583790 STATS ;
Vagrant 2.4.0 ;

Linux 6.1.0-13-arm64 #1 SMP Debian 6.1.55-1 (2023-09-29) GNU/Linux Description:	Debian GNU/Linux 12 (bookworm) ;
Vault v1.15.4+ent (780d9977265396e561741bd9045cf1f673a5228a), built 2023-12-05T01:50:21Z;
```


## Host Machine - To Run:

```bash
VD=$(date "+%F @ %H:%M") ;
# // brew install inxi # OR apt-get install inxi
VOS=$(uname -orsv) ; if [[ ${VOS} == *"Darwin"* ]] ; then VOS+=" macOS $(sw_vers | grep 'ProductVersion')" ; elif [[ ${VOS} == *"Linux"* ]] ; then VOS+=" $(lsb_release -a | grep Description)" ; fi ;
VVBOX="$(/Applications/VMware\ Fusion.app/Contents/Library/vmware-vmx-stats -v 2>&1)" ;
VVAGR="$(vagrant --version)" ;
VCPU=$(inxi -c 2>/dev/null | grep CPU) ;
printf "\n$VD ;\n${VOS} ;\n${VCPU};${VVBOX} ;\n${VVAGR} ;\n\n" ;
```


## VM Node - To Run:

```bash
vagrant ssh vault1 -c 'VOS=$(uname -orsv) ; VOS+=" $(lsb_release -a 2>/dev/null | grep Description)" ; VVAULT="$(vault --version)" ; printf "\n${VOS} ;\n${VVAULT};\n\n" ;'
```
