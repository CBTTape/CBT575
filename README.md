# CBT575
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 575 is from Thomas Hutchins, and contains a JES2 spool    *   FILE 575
//*           offload utility which allows you to read the data     *   FILE 575
//*           directly off the JES2 SPOOL OFFLOAD datasets, so      *   FILE 575
//*           you don't have to restore it on a JES2 system first,  *   FILE 575
//*           to retrieve the data.  The program is called          *   FILE 575
//*           SPOOLSEL, and it has some other utility capabilities  *   FILE 575
//*           having to do with JES2 SPOOL OFFLOAD files, on tape   *   FILE 575
//*           or on disk.                                           *   FILE 575
//*                                                                 *   FILE 575
//*           Also included are 3 edit macros which allow quick     *   FILE 575
//*           translation and viewing of data from ASCII to EBCDIC, *   FILE 575
//*           EBCDIC to ASCII, and EXAMHTM, which creates a         *   FILE 575
//*           temporary file for safety, when you use ASCEBC.       *   FILE 575
//*                                                                 *   FILE 575
//*           Descriptions of these macros are written below the    *   FILE 575
//*           following descriptions for SPOOLSEL.                  *   FILE 575
//*                                                                 *   FILE 575
//*    Quick Description of SPOOLSEL:                               *   FILE 575
//*                                                                 *   FILE 575
//*      I am sending you 2 files which contain an updated          *   FILE 575
//*      "SHARE" utility which allows you to list what is on a      *   FILE 575
//*      JES2 offloaded data set, print any or all the contents     *   FILE 575
//*      of a JES2 offloaded data set or create a JES2 offloaded    *   FILE 575
//*      data set from multiple JES2 offloaded data sets.           *   FILE 575
//*                                                                 *   FILE 575
//*      We use this to store our product installation printouts    *   FILE 575
//*      to offloaded tapes.  This spares us from keeping paper     *   FILE 575
//*      copies and does not use space in the system report         *   FILE 575
//*      archiver.  The utility is known as "SPOOLSEL" on the       *   FILE 575
//*      SHARE site.  I modified their code to work in an OS/390    *   FILE 575
//*      environment.                                               *   FILE 575
//*                                                                 *   FILE 575
//*      Thomas Hutchins                                            *   FILE 575
//*      Sr. OS/390 Systems Programmer                              *   FILE 575
//*                                                                 *   FILE 575
//*      email:  thutchns@earthlink.net                             *   FILE 575
//*                                                                 *   FILE 575
//*   - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - *   FILE 575
//*                                                                 *   FILE 575
//*    Detailed Description:                                        *   FILE 575
//*                                                                 *   FILE 575
//*    NAME:                SPOOLSEL                                *   FILE 575
//*                                                                 *   FILE 575
//*    RELATED SYSTEM:      JES2/NJE Spool Offloader                *   FILE 575
//*                                                                 *   FILE 575
//*    PURPOSE:             Multi-purpose offload utility.          *   FILE 575
//*                         1. List conents of offloaded dataset    *   FILE 575
//*                         2. Print selected members of dataset    *   FILE 575
//*                         3. Create subset of offloaded dataset   *   FILE 575
//*                                                                 *   FILE 575
//*    TYPE OF LANGUAGE:    ASSEMBLER                               *   FILE 575
//*    SOURCE NAME:         SPOOLSEL                                *   FILE 575
//*                                                                 *   FILE 575
//*    SOURCE LOCATION:     userid.FILE575.PDS                      *   FILE 575
//*    MACRO LOCATION:      userid.FILE575.PDS(MACLIB)              *   FILE 575
//*    OBJECT NAME:         n/a                                     *   FILE 575
//*    OBJECT LOCATION:     n/a                                     *   FILE 575
//*    LOAD NAME:           SPOOLSEL                                *   FILE 575
//*    LOAD LOCATION:       SYS2.LINKLIB                            *   FILE 575
//*    PROC MEMBERS:        n/a                                     *   FILE 575
//*    PROC LOCATION:       n/a                                     *   FILE 575
//*    COMPILE/ASSEMBLE:    member SPOOLASM                         *   FILE 575
//*                                                                 *   FILE 575
//*    COMP/ASSMBL OPTS:                                            *   FILE 575
//*             SYSPARM((NOGEN,NOGEN,NODATA,NOGEN,NOGEN,240))       *   FILE 575
//*                                                                 *   FILE 575
//*    LINKAGE EDITOR OPTS: AMODE(24), RMODE(24)                    *   FILE 575
//*                                                                 *   FILE 575
//*    FILES INVOLVED:      SYSUT1   - defining input data set,     *   FILE 575
//*                                    (offloaded spool data        *   FILE 575
//*                                    set) mandatory in all        *   FILE 575
//*                                    modes.                       *   FILE 575
//*                         SYSPRINT - defining printout data       *   FILE 575
//*                                    set (messages and list       *   FILE 575
//*                                    info) mandatory in all       *   FILE 575
//*                                    modes.                       *   FILE 575
//*                         PRNT???? - dynamically allocated        *   FILE 575
//*                                    printout data sets which     *   FILE 575
//*                                    will receive selected        *   FILE 575
//*                                    jobs outputs in print        *   FILE 575
//*                                    mode.                        *   FILE 575
//*                         SYSIN    - defining input data set      *   FILE 575
//*                                    (select control cards)       *   FILE 575
//*                                    mandatory in select and      *   FILE 575
//*                                    print                        *   FILE 575
//*                         SYSUT2   - defining output data set     *   FILE 575
//*                                    (offloaded spool sub-set)    *   FILE 575
//*                                    mandatory in select mode.    *   FILE 575
//*                                                                 *   FILE 575
//*    PARM CHANGES:        HEX    - hex print of blocks headers    *   FILE 575
//*                         LIST   - list the contents of the       *   FILE 575
//*                                  spool offload data set         *   FILE 575
//*                         MERGE  - merge the contents of spool    *   FILE 575
//*                                  offload data sets              *   FILE 575
//*                         PRINT  - print the selected jobs        *   FILE 575
//*                                  outputs onto printout data     *   FILE 575
//*                                  set                            *   FILE 575
//*                         SELECT - select the requested jobs      *   FILE 575
//*                                  creating selective copy        *   FILE 575
//*                                                                 *   FILE 575
//*                         Absence of the parm field implies       *   FILE 575
//*                         list option only.                       *   FILE 575
//*                                                                 *   FILE 575
//*    EXECUTION JCL:       member   SPOOLXHE                       *   FILE 575
//*                         member   SPOOLXLS                       *   FILE 575
//*                         member   SPOOLXMG                       *   FILE 575
//*                         member   SPOOLXPR                       *   FILE 575
//*                         member   SPOOLXSL                       *   FILE 575
//*                                                                 *   FILE 575
//*    SPECIAL CONDITIONS:  May require re-assembly when            *   FILE 575
//*                         changing versions of JES2.              *   FILE 575
//*                                                                 *   FILE 575
//*    IN-DEPTH INFO:       Program reads an NJE formatted spool    *   FILE 575
//*                         offload data set and processes it       *   FILE 575
//*                         according to the JCL parameter and      *   FILE 575
//*                         control cards.  A listing of the        *   FILE 575
//*                         contents of the dataset provide job     *   FILE 575
//*                         name and number of every member.        *   FILE 575
//*                         Print provides a printout of            *   FILE 575
//*                         selective or all members on the         *   FILE 575
//*                         offload dataset.  All printouts are     *   FILE 575
//*                         divided such that each printout         *   FILE 575
//*                         occupies a separate SYSOUT.  Select     *   FILE 575
//*                         allows a subset of the offload          *   FILE 575
//*                         dataset to be built.                    *   FILE 575
//*                                                                 *   FILE 575
//*    PRODUCTION DATE:     June 28,1999                            *   FILE 575
//*                                                                 *   FILE 575
//*   - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - *   FILE 575
//*                                                                 *   FILE 575
//*      From the original note I sent the other System             *   FILE 575
//*      Programmers:  This is one recommendation for how           *   FILE 575
//*      SPOOLSEL can be set up and used profitably.                *   FILE 575
//*                                                                 *   FILE 575
//*      I would like you to consider the following as a standard   *   FILE 575
//*      when installing a new product or applying maintenance to   *   FILE 575
//*      an existing one.  We get overwhelmed by the amount of      *   FILE 575
//*      printout that an install uses.  Some of the printouts      *   FILE 575
//*      will get printed and filed, but the rest?  I have tested   *   FILE 575
//*      the following on many installs.                            *   FILE 575
//*                                                                 *   FILE 575
//*      1. Provide all of your install jobs with a JOBPARM         *   FILE 575
//*         "forms" statement.  If you forget, don't worry. You     *   FILE 575
//*         can use IOF to change the forms name.  Use a relevant   *   FILE 575
//*         form for your product: i.e. ( PANV, Panvalet / SYNC,    *   FILE 575
//*         Syncsort / SMPE, Operating System ).  LINES=9999        *   FILE 575
//*         will stop annoying "ESTIMATED LINES EXCEEDED"           *   FILE 575
//*         messages.                                               *   FILE 575
//*                                                                 *   FILE 575
//*            /*JOBPARM LINES=9999,FORMS=????                      *   FILE 575
//*                                                                 *   FILE 575
//*      2. Send all of the installation printout to RMT2.  You     *   FILE 575
//*         need it to hang around.  And don't use class Q, it      *   FILE 575
//*         disappears in 3 days.  Class T will stay.               *   FILE 575
//*                                                                 *   FILE 575
//*            //jobname JOB (6948),MSGCLASS=T                      *   FILE 575
//*            /*ROUTE PRINT RMT2                                   *   FILE 575
//*                                                                 *   FILE 575
//*      3. When the install or maintenance is finished, use        *   FILE 575
//*         JES2 commands or IOF to off load the listings based     *   FILE 575
//*         on the JES2 "form" id to tape (3490) and using this     *   FILE 575
//*         data set naming convention:                             *   FILE 575
//*                                                                 *   FILE 575
//*         SFT1.JES2OFFL.prod.vers.Dyyddd                          *   FILE 575
//*                                                                 *   FILE 575
//*          prod    -  short form for product generally 4 chars    *   FILE 575
//*          vers    -  version reference                           *   FILE 575
//*          yyddd   -  julian year and date                        *   FILE 575
//*                                                                 *   FILE 575
//*      4. After off loading the tape, go to CA-1 and update       *   FILE 575
//*         the tape volser or data set name to an expiration       *   FILE 575
//*         period of CATALOG.  JES2 will not allow retpd of        *   FILE 575
//*         99000 for CA-1.                                         *   FILE 575
//*                                                                 *   FILE 575
//*      The first 2 qualifiers will allow anyone immediate         *   FILE 575
//*      access to off loaded listings for any product via DSN      *   FILE 575
//*      or ISPF 3.4 wild carding without doing extensive           *   FILE 575
//*      catalog searches.                                          *   FILE 575
//*                                                                 *   FILE 575
//*      You can IEBGENER the tape to another name to provide a     *   FILE 575
//*      backup copy.  Still cheaper than paper or filling the      *   FILE 575
//*      $AVRS file.                                                *   FILE 575
//*                                                                 *   FILE 575
//*      The listings can be "received" back into the JES2 spool    *   FILE 575
//*      at any time you need to check something. You always        *   FILE 575
//*      have access to the listings without keeping mounds of      *   FILE 575
//*      paper.                                                     *   FILE 575
//*                                                                 *   FILE 575
//*      I have found and updated a program, SPOOLSEL, that can     *   FILE 575
//*      read the tape and list or print its contents.  See the     *   FILE 575
//*      documentation in:  userid.FILE575.PDS($$DOC).              *   FILE 575
//*                                                                 *   FILE 575
//*      - - - - - - - - - - - - - - - - - - - - - - - - - - - -    *   FILE 575
//*                                                                 *   FILE 575
//*    Description of ASCII <-> EBCDIC edit macros:                 *   FILE 575
//*                                                                 *   FILE 575
//*      I am sending you several REXX edit macros that I have      *   FILE 575
//*      found useful in dealing with the Unix System Services      *   FILE 575
//*      file system.  We have several USS Datasets from which      *   FILE 575
//*      our Dealers download HTML code to be displayed on their    *   FILE 575
//*      connected PCs.  These files are kept in ASCII format       *   FILE 575
//*      and some times I have been called upon to view these       *   FILE 575
//*      files through TSO/ISPF OEDIT or ISH utilities.  These      *   FILE 575
//*      macros have made it unnecessary to FTP to a PC and look    *   FILE 575
//*      at it there.  The following REXX Edit macros make it       *   FILE 575
//*      easy to translate from ASCII to EBCDIC and back.           *   FILE 575
//*                                                                 *   FILE 575
//*      1: EXAMHTM - creates a temporary file that is translated   *   FILE 575
//*                   to EBCDIC.  It depends on ASCEBC macro.       *   FILE 575
//*                   Builds a temporary file for safety.           *   FILE 575
//*                                                                 *   FILE 575
//*      2: ASCEBC  - translates the edited file to EBCDIC.         *   FILE 575
//*                                                                 *   FILE 575
//*      3: EBCASC  - translates the edited file to ASCII.          *   FILE 575
//*                                                                 *   FILE 575
//*      Note that these macros do not check the actual file        *   FILE 575
//*      type.  If the file is already ASCII and you use EBCASC     *   FILE 575
//*      against it, you have just encrypted and possibly           *   FILE 575
//*      corrupted it.                                              *   FILE 575
//*                                                                 *   FILE 575
//*      Thomas Hutchins                                            *   FILE 575
//*      AGCO Corp                                                  *   FILE 575
//*      1500 N Raddant RD                                          *   FILE 575
//*      Batavia, IL                                                *   FILE 575
//*      (630) 406-3312                                             *   FILE 575
//*      thutchns@earthlink.net                                     *   FILE 575
//*                                                                 *   FILE 575
```
