# 1. i-like-logic-more
idk man, i feel like microsd cards are a thing of the past.
## Solution:

## Flag: 

## Concepts Learnt: 

## Notes:

## Resources: 

# 2. Red-Devil
this is the worst football team ever(i dont even watch football lmeow)

## Solution:
started with a quick google search to find what a .cf32 file is(complex float 32 bit binary data). i learnt this was used in Software-Defined Radio (SDR) and signal processing. some quick looking around and i came across ```rtl_433```. a software which can read this type of file.
running the program i got ->

```
 rtl_433 signal.cf32 -A
rtl_433 version 21.12 (2021-12-14) inputs file rtl_tcp RTL-SDR SoapySDR
Use -h for usage help and see https://triq.org/ for documentation.
Trying conf file at "rtl_433.conf"...
Trying conf file at "/home/memeboichatur/.config/rtl_433/rtl_433.conf"...
Trying conf file at "/usr/local/etc/rtl_433/rtl_433.conf"...
Trying conf file at "/etc/rtl_433/rtl_433.conf"...
Registered 176 out of 207 device decoding protocols [ 1-4 8 11-12 15-17 19-23 25-26 29-36 38-60 63 67-71 73-100 102-105 108-116 119 121 124-128 130-149 151-161 163-168 170-175 177-197 199 201-207 ]
Test mode active. Reading samples from file: signal.cf32
baseband_demod_FM_cs16: low pass filter for 250000 Hz at cutoff 25000 Hz, 40.0 us
Detected OOK package    @0.220228s
Analyzing pulses...
Total count:  185,  width: 1837.12 ms           (459281 S)
Pulse width distribution:
 [ 0] count:  114,  width: 3608 us [3604;3624]  ( 902 S)
 [ 1] count:   71,  width: 7204 us [7200;7208]  (1801 S)
Gap width distribution:
 [ 0] count:   71,  width: 7172 us [7172;7180]  (1793 S)
 [ 1] count:  113,  width: 3576 us [3576;3584]  ( 894 S)
Pulse period distribution:
 [ 0] count:   57,  width: 10784 us [10780;10796]       (2696 S)
 [ 1] count:   42,  width: 14380 us [14376;14384]       (3595 S)
 [ 2] count:   85,  width: 7188 us [7184;7196]  (1797 S)
Pulse timing distribution:
 [ 0] count:  227,  width: 3592 us [3576;3624]  ( 898 S)
 [ 1] count:  142,  width: 7188 us [7172;7208]  (1797 S)
 [ 2] count:    1,  width: 72084 us [72084;72084]       (18021 S)
Level estimates [high, low]:  15985,    488
RSSI: -0.2 dB SNR: 30.3 dB Noise: -30.5 dB
Frequency offsets [F1, F2]:   -5928,      0     (-22.6 kHz, +0.0 kHz)
Guessing modulation: Manchester coding
view at https://triq.org/pdv/#AAB1030E081C14FFFF819191919191919191919191919191918080808090818080918090808180918091808080919191808091808080918090808081908191918091809180809081809190808080819180918080808090819180809081808090819081919081809081808091908190808180809081908180919080808081809081808091908081809081919080808081908180809081809081808080808090818080808090819081808080918080809180918080809180918080809190808080819255
Attempting demodulation... short_width: 3608, long_width: 0, reset_limit: 7184, sync_width: 0
Use a flex decoder with -X 'n=name,m=OOK_MC_ZEROBIT,s=3608,l=0,r=7184'
pulse_demod_manchester_zerobit(): Analyzer Device
bitbuffer:: Number of rows: 1
[00] {256} 2a aa aa aa 0c 4e 48 54 42 7b 52 46 5f 48 34 63 6b 31 6e 36 5f 31 73 5f 63 30 30 6c 21 21 21 7d

```
decoding it i found a hex string. using cyberchef i was able to get the flag.

## Flag: 
NHTB{RF_H4ck1n6_1s_c00l!!!}


## Concepts Learnt: 
RF Protocols
Usage of rtl_433

## Notes:

## Resources: 

# 3. Formwear 
this is most definitely going to ring some bells for those who attended the "router hijacking" workshop L0L

## Solution:
started with unzipping the .zip file to get a lot of files. i focussed on the rootfs file.
running ```file rootfs``` gave me ```  
file rootfs
rootfs: Squashfs filesystem, little endian, version 4.0, zlib compressed, 10936182 bytes, 910 inodes, blocksize: 131072 bytes, created: Sun Oct  1 07:02:43 2023
```
searching around, i learnt squashfs is a way of compressing files, and can be uncompressed using ```  unsquashfs rootfs ```
doing this
```
create_inode: could not create character device squashfs-root/dev/zero, because you're not superuser!
[======================================================================================================================-                                   ]  807/1045  77%

created 440 files
created 45 directories
created 187 symlinks
created 0 devices
created 0 fifos
created 0 sockets
```

gave me a folder called squashfs-root. looking around i found some text strings hidden. one said "youre close". i figured the flag might be hardcoded directly here. using ``` grep -rnE "[A-Z0-9_]+\{.*\}" squashfs-root/ ```
i got 
```grep: squashfs-root/bin/dnsmasq: binary file matches
grep: squashfs-root/bin/samba_multicall: binary file matches
grep: squashfs-root/bin/systemd: binary file matches
squashfs-root/etc/config_default.xml:244:<Value Name="SUSER_PASSWORD" Value="HTB{N0w_Y0u_C4n_L0g1n}"/>
squashfs-root/etc/dhclient-script:38:  dslite_enable=$(cat /var/dhcpcV6${interface}.leases | grep dslite | awk '{print $(NF)}' | awk 'END{print}' | tr -d ';' | tr -d '\"' | head -c -2)
grep: squashfs-root/lib/libcrypto.so.1.1: binary file matches
grep: squashfs-root/lib/modules/4.4.140/kernel/drivers/net/ethernet/realtek/rtl86900/igmpHookModule/rtk_igmp_hook.ko: binary file matches
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp2/challenge_files/hardware/formware$ file rootfs
rootfs: Squashfs filesystem, little endian, version 4.0, zlib compressed, 10936182 bytes, 910 inodes, blocksize: 131072 bytes, created: Sun Oct  1 07:02:43 2023
memeboichatur@xenon:/mnt/c/Users/chatu/OneDrive/Desktop/jtp2/challenge_files/hardware/formware$
```

the flag was hardcoded inside the files and i was able to retrieve it this way, using grep.

## Flag: 
HTB{N0w_Y0u_C4n_L0g1n}

## Concepts Learnt: 
SquashFS Compression

## Notes:

## Resources: 


# 4. Speed Thrills but Kills
i recently got involved in a hit and run case in pune, that kids porsche was going wayy too fast, if only i knew what the VIN of the car was :(

## Solution:
extracting the .zip file, i got a .sal file. some looking around, i learnt this can be opened with the Logic 2 software, which i downloaded next. opening the software and loading the file into it, I attempted to decode the signal as UART with various bitrates(incorrect tangent). after some struggling here, i looked at the challenge description again.
some more searching made me realise cars use the CAN protocol.
i selected the CAN protocol in the analyzer. my next task was to figure out the baud rate. as standard 500000 and 1000000 didnt work, i used the measuring tool and got 125,000 bits/s. loading all this information up, the data text showed hex ``` 48 54 42 7b 76 31 6e 5f ```, which when converted to ascii gave ```HTB{v1n_```


## Flag: 
HTB{v1n_
## Concepts Learnt: 

## Notes:

## Resources: 


# 5. Gates Of Mayhem
iqtest but its on steriods and you have weird aah inputs aswell.

## Solution:

## Flag: 

## Concepts Learnt: 

## Notes:

## Resources: 