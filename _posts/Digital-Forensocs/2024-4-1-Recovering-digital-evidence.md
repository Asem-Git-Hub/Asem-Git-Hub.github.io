---
title: " Recovering Digital Evidence "
classes: wide
header:
  teaser: /assets/images/digital-forensics/recovering_digital-evidence/main.png
ribbon:
description: " Tracing Digital Footprints: Insights into File Recovery and Data Carving"
categories:
  - digital-forensics
toc: true
---


# Recovering Erased Digital Evidence:

   In the world of computers and technology, retrieving erased digital evidence is a complex task that demands a deep understanding of how computers work and specialized investigation methods.
   Forensic experts use various tools and methods, such as examining file systems and using advanced techniques to piece together hidden information from digital files. In this detailed exploration, we delve into the process of recovering deleted digital evidence, highlighting the tools, methods, and technical factors that drive investigations in today's digital era.

   Digital evidence refers to information stored or sent in digital format that can be presented in court during a trial. It includes things like audio files, voice recordings, address books, backups of programs (even on mobile devices), browsing history, cookies, databases, and compressed archives (like ZIP or RAR files), including encrypted ones.

  ![error](/assets/images/digital-forensics/recovering_digital-evidence/case_1.png)

## Destroyed Evidence:

   In criminal or cyber-criminal cases, it's common for attempts to be made to destroy evidence. The success of these attempts depends on a few factors:

 -	The action taken to destroy the evidence.
 -	The amount of time available to destroy the evidence.
 -	The type of storage device being used, such as a magnetic hard drive, flash memory card, or SSD drive.

   **In this section, we'll explore some methods used to destroy evidence and ways to recover evidence that has been destroyed.**

   Digital evidence encompasses a vast array of data types, each governed by distinct data structures and storage mechanisms. From file systems like <u>NTFS</u> and <u>FAT</u> to database formats like <u>SQLite</u> and <u>MySQL</u>, understanding the intricacies of digital data structures is paramount for forensic practitioners. Tools like <u>FTK Imager</u> and <u>Autopsy</u> facilitate the analysis of digital evidence, allowing investigators to extract valuable artifacts from storage media and reconstruct digital timelines with precision.

  ![error](/assets/images/digital-forensics/recovering_digital-evidence/ftk_image.png)

## Eradicated Evidence: Overcoming Data Destruction

   Criminals frequently try to hide their actions by deleting or altering digital evidence, making it difficult for forensic investigators. Nonetheless, tools such as <u>EnCase</u> Forensic and <u>X-Ways</u> Forensics empower practitioners to retrieve deleted files and artifacts by examining file system metadata and disk sectors. Methods like file carving and signature analysis assist in reconstructing digital artifacts, enabling investigators to unravel the intricacies of data destruction with precision.

## Deleted Files: Tracing Digital Footprints:

   <u>The principle of file recovery of deleted files is based on the fact that Windows does not wipe the contents of the file when it’s being deleted.</u>

   The deletion of files leaves behind digital footprints that can be traced and reconstructed using specialized tools and techniques. Tools like <u>Recuva</u> and <u>PhotoRec</u> facilitate the recovery of deleted files by scanning storage media for remnants of data clusters and file headers. 

  ![error](/assets/images/digital-forensics/recovering_digital-evidence/rec_photo.png)

   Forensic experts utilize file system journaling and metadata analysis to pinpoint and restore deleted files, meticulously reconstructing digital trails with precision and accuracy. **file system record storing the exact location of the deleted file on the disk is being marked as deleted and the disk space previously occupied by the deleted file is then labeled as available – but not overwritten with zeroes or other data.** 

   By examining characteristic signatures of known file types through file system analysis or scanning the entire hard drive, one can successfully recover:

   - Files that were intentionally deleted by the user.
   - Temporary copies of Office documents, including previous versions and revisions.
   - Temporary files created by various applications.
   - Files that have been renamed.

   Moreover, information stored in deleted files can be supplemented with data retrieved from other sources. For instance, Skype's "chatsync" folder retains internal data that may contain fragments of user conversations. This means that even if the Skype database is deleted, there's a chance to recover user chats if the "chatsync" folder exists. Several tools, such as <u>Belkasoft Evidence Center</u>, are available for this purpose.

## Formatted Hard Drives: Unraveling File System Structures

   The ability to recover data from a formatted hard drive depends on several factors. Data from a formatted hard drive may be retrievable either through data carving technology or by using commercial data recovery tools. There are two primary methods of formatting a hard drive:

   **Full Format:**
    This process initializes the disk by creating a new file system on the partition being formatted and checks the disk for bad sectors. In older versions of Windows (prior to Vista), a full format operation didn't zero the disk, instead, it scanned the disk surface sector by sector and marked unreliable sectors as bad. However, in Vista and Windows 7, a full-format operation will:
  
   - Wipe the disk clean.
   - Write zeroes onto the disk.
   - Read the sectors back to ensure reliability.
   
   **Quick Format:** 
   This method is non-destructive except in the case of SSDs. It simply initializes the disk by creating a new file system on the partition being formatted. Data from disks cleared using a quick format method can often be recovered using data recovery tools that support data carving.


## SSD Conundrum: Navigating Flash Storage

   Solid-State Drives (SSDs) are a newer storage technology known for their speed and different internal storage methods compared to traditional drives. Key points about SSDs include:

   - Speed: SSDs operate much faster than traditional hard drives.

   - Data Storage Method: They utilize a distinct internal storage method, making it easier to destroy data and harder to recover it.
  
   - TRIM Command: TRIM is a feature in SSDs that swiftly wipes deleted information, typically within three minutes. Once data is marked as deleted by the operating system, TRIM zeros out the information, and its effects cannot be blocked even by Write-Blocking devices.

  ![error](/assets/images/digital-forensics/recovering_digital-evidence/trim.png)

  `NTFS DisableDeleteNotify = 0`  means that the TRIM feature is enabled on your SSD.
  
  `NTFS DisableDeleteNotify = 1` means that the TRIM feature is disabled on your SSD.
  
   When attempting to recover data from SSDs or even accessing information from SSDs formatted with either a Full or Quick format, traditional methods are generally ineffective. Traditional data recovery methods can only be utilized on SSDs if the TRIM command is not executed or if at least one of the components does not support TRIM. These components include:
  
   - Operating System Version: Windows Vista and Windows 7 support TRIM, while Windows XP and earlier versions usually do not.
   - Communication Interface: `SATA` and `eSATA` interfaces support TRIM, while external enclosures connected via USB, LAN, or FireWire typically do not.
   - File System: Windows supports `TRIM` on `NTFS` volumes but not on FAT-formatted disks. Linux supports TRIM on all volume types, including those formatted with FAT.

     
## Unveiling Data Carving: Extracting Digital Fragments
   Data carving techniques enable forensic practitioners to extract digital fragments from unallocated disk space, reconstructing deleted files with precision and accuracy. Tools like <u>Scalpel</u> and <u>Foremost</u> facilitate the extraction of file signatures and data patterns from disk images, enabling investigators to recover deleted files and artifacts. 

  ![error](/assets/images/digital-forensics/recovering_digital-evidence/foremost.png)


   Advanced techniques such as entropy analysis and file header reconstruction aid in the identification and reconstruction of fragmented data, allowing practitioners to piece together the puzzle of digital evidence with meticulous detail.

   Carving involves a meticulous examination of the entire content of a hard drive, bit by bit and in sequence. It differs from file recovery and enables:
   
   - Identification of Signatures or Patterns: This helps in spotting potentially interesting data stored in specific areas of the disk.
   
   - Discovery of Artifacts: Carving can locate various artifacts that may not be accessible through other means.

   When seeking destroyed evidence, investigators find Data Carving invaluable. Unlike file recovery, it doesn't rely on intact files, as they might be partially overwritten or fragmented across the disk. Data Carving has distinct features for text and binary data:

**Text Content:**
   - Text data is relatively easier to recover.
   - Text blocks contain numeric values representing letters, numbers, and symbols within a narrow range.
   - Different languages and text encodings must be considered during analysis.

**Binary Data:**
   - Binary data appears more random.
   - Text block boundaries can be determined by counting non-language-specific characters.

However, Data Carving has limitations:
   - Not all data formats can be carved.
   - Some files lack permanent signatures in their headers.
   - Data Carving is ineffective against special algorithms that fill disk space with random data.
   - It's impossible to use Data Carving on SSDs due to their operation and structure.

In scenarios where sensitive information is stored in RAM instead of on a hard drive, Live RAM Analysis becomes the only feasible option.


























