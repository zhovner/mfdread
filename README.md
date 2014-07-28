Mifare dumps parser 
=======

Mifare Classic 1k and 4k dumps parser in human readable format. 
Dumps can be grabbed by mfoc or nfc-mfclassic tools from libnfc.org
Dump file size must be 4096 bytes.

Included ```dump.mfd``` -- Mifare 4k dump for testing.

#### Usage:
```mfdread.py ./dump.mfd```

![Mifare mfd dump parser](https://zhovner.com/forever/mfdread.png)

The total memory of 1024 bytes in Mifare Classic (1k) and 4095 bytes in Mifare 4k is divided into 16 sectors of 64 bytes, each of the sectors is divided into 4 blocks of 16 bytes. Blocks 0, 1 and 2 of each sector can store data and block 3 is used to store keys and access bits (the exception is the ‘Manufacturer Block’ which can not store data).

![Mifare memory structure](https://zhovner.com/forever/MiFare_Memory_Structure.png)

The memory of 1KB and 4KB MIFARE Classic cards is ordered in a similar way. On both cards the first block (block 0) contains the UID, BCC, SAK, ATQA and Manufacturer data. This block is locked and cannot be altered. But some times it can be ;)

![Mifare zero block structure](https://zhovner.com/forever/0blockmifare.gif)

Abbreviation  | Meaning 
------------- | -------------
UID  | Unique Identifier, Type A
NUID  | Non-Unique Identifier
ATQA  | Answer To Request acc. to ISO/IEC 14443-4
SAK  | Select Acknowledge, Type A
ATS  | Answer To Select acc. to ISO/IEC 14443-4
ATR  | Anser To Reset [What's really ATR means](#ATR)
DIF  | Dual Interface (cards)
COS  | Card Operating System
CL  | Cascade Level acc. to ISO/IEC 14443-3
CT  | Cascade Tag, Type A
NFC  | Near Field Communication
PCD  | Proximity Coupling Device (“Contactless Reader”)
PICC  | Proximity Integrated Circuit (“Contactless Card”)
PKE  | Public Key Encryption (like RSA or ECC)
REQA  | Request Command, Type A
Select  | Select Command, Type A
RID  | Random ID, typically dynamically generated at Power-on Reset (UID0 = “0x08”, Random number in UID1… UID3)
RFU  | Reserved for future use



### <a name="ATR"></a>What's really ATR means:
ATR is for contact cards and is specified in ISO 7816. For contacless cards, it is the PC/SC reader (IFD) that generates the ATR.

The ATR is constructed based on:

ATS (Answer to Select) for ISO 14443 Type A cards
ATQB and ATTRIB bytes for ISO 14443 Type B cards
The ATR will be of the form 3B 8X 80 01 HB_ATS Parity_Byte where X is the number of bytes of Historical Bytes of ATS (HB_ATS).

The exact construction of ATR for contactless cards is given in section 3.1.3.2.3 of the PC/SC spec.

Given that the only variable is ATS, it should be the same regardless of the reader.






