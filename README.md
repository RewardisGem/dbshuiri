


# DBSHUIRI DSR 1.1 Documentation

## Introduction

DBSHURI DSR is a tool for quick fix oracle datafile.

DSR is a command line tool , it can be used to edit oracle datafile header , and then recover the database.

 ### Core Functionality:
DbShuiRi (DSR) is designed to address Oracle datafile issues by providing a command-line interface for users to execute various commands targeting the datafile header to facilitate database recovery.

### Key Commands and Operations:
- **Backup and Restore Datafile Header**: 
  - `backuphdr`: This command backs up the datafile header.
  - `restorehdr`: This command restores the datafile header from a backup.
- **Manipulate Checkpoint SCN**:
  - `ckpscn`: This command sets the checkpoint System Change Number (SCN) for a datafile, which is crucial for maintaining data consistency in Oracle databases.
- **View Datafile Header Content**:
  - `hdr`: This command displays the content of the datafile header, providing insight into the crucial metadata stored in the header.

### Application in Real-world Scenarios:
- **Lost Current Redo Log File Scenario**:
   - When the current redo log file is lost, Oracle will throw errors (`ORA-00313` and `ORA-00312`) upon attempting to open the database. 
   - An attempt to open the database with resetlogs (`alter database open resetlogs;`) results in error `ORA-01139` since this operation is only valid after an incomplete database recovery.
   - The commands provided by DSR can be used to rectify the header information, set the appropriate SCN values, and proceed with the recovery operations to get the database back online.


- **CASE Oracle Datafile Damaged by Malware/Ransomware -- devos**:
   - In situations where a datafile is compromised by malware or ransomware, DSR can aid in recovery. 
   - The `dsr initfake` command can be used to create a good datafile header, essentially faking a healthy state for the datafile.
   - Post this, the `dsr fitsize` command can be used to adjust the datafile header to fit the real size of the datafile, aiding in the restoration of the database.


### Working Principles:
1. **Datafile Header Manipulation**: 
   - The primary principle behind DSR's operation is manipulating Oracle datafile headers. By modifying certain header values like the checkpoint SCN, DSR helps to overcome inconsistencies that prevent a database from starting up or functioning correctly.
2. **Command-line Interface**: 
   - DSR operates through a command-line interface, providing a suite of commands for precise control over datafile header values and other datafile properties. This CLI approach allows for targeted interventions to resolve specific issues with Oracle datafiles.

### Technical Details:
- DSR provides a variety of commands to handle different aspects of datafile and database management, including setting/checking values like the checkpoint SCN, creation SCN, database ID, and more.
- It also offers commands for block-level operations, marking datafile blocks as corrupt, summing datafile blocks, and more, providing a comprehensive toolkit for database administrators in the recovery and maintenance of Oracle databases.

### Summary:
DbShuiRi (DSR) is a powerful tool for Oracle database administrators, offering a granular level of control over datafile headers, which are crucial for the operation and recovery of Oracle databases. Through a set of well-defined commands, DSR facilitates the manipulation of key datafile attributes, aiding in the recovery of databases from various error states and corruption scenarios.


 




    $ dsr
    dbshuri tool for quick fix oracle datafile
    Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
    Free for personal use; commercial users need a license.
    
    Usage:
    dsr [command]
    
    Available Commands:
    backuphdr   backup datafile header
    ckpscn      set checkpoint scn for datafile
    completion  Generate the autocompletion script for the specified shell
    copy        copy block
    copyany     copyany block
    crsscn      set creation scn for datafile
    crt         set crt for a datafile
    csq         set csq for a datafile
    cvn         Set cvn
    damage1     damage a file
    damage2     damage a file
    dbi         set dbid for datafile
    dbname      set dbname for datafile
    dbv         check a datafile
    edit        edit ub4 datafile
    fill        fill a datafile with size
    fitsize     set fit size
    fno         set fno for a datafile
    fsz         set fsz for a datafile
    fuzz        set fuzz for datafile
    guess       guess block size
    hdr         show datafile header content
    help        Help about any command
    initfake    initfake for a datafile
    makerdba    make rdba
    markcorrupt markcorrupt datafile
    mt          make template
    parserdba   Parse Rdba
    rdba        set rdba for datafile
    register    register license key
    restorehdr  restore datafile header from backup
    rfn         set rfn for a datafile
    rlc         set resetlogs counter for datafile
    rlsscn      set resetlogs scn for datafile
    rt          read Template
    sum         sum datafile
    sumapply    sumapply datafile 10
    test        test
    tsn         set tsn for datafile
    tsname      set tsname for datafile
    version     Print version info
    wt          write template
    
    Flags:
    --blocksize int   The BLOCKSIZE can be 8192,16384,32768.
    --endian string   The ENDIAN can be BIG,LITTLE.
    -h, --help            help for dsr
    
    Use "dsr [command] --help" for more information about a command.





## CASE 1 Lost Current Redo Log File


    SQL> alter database open;
    alter database open
    *
    ERROR at line 1:
    ORA-00313: open failed for members of log group 3 of thread 1
    ORA-00312: online log 3 thread 1:
    '/opt/oracle/oradata/OLTP/onlinelog/o1_mf_3_ld9ronjn_.log'
    ORA-27037: unable to obtain file status
    Linux-x86_64 Error: 2: No such file or directory
    Additional information: 3

    SQL> alter database open resetlogs;
    alter database open resetlogs
    *
    ERROR at line 1:
    ORA-01139: RESETLOGS option only valid after an incomplete database recovery
    
    
    SQL> recover database until cancel;
    ORA-00279: change 1361398 generated at 08/21/2023 06:23:38 needed for thread 1
    ORA-00289: suggestion : /d02/fra/OLTP/archivelog/2023_08_21/o1_mf_1_36_%u_.arc
    ORA-00280: change 1361398 for thread 1 is in sequence #36
    
    
    Specify log: {<RET>=suggested | filename | AUTO | CANCEL}
    cancel
    ORA-01547: warning: RECOVER succeeded but OPEN RESETLOGS would get error below
    ORA-01194: file 1 needs more recovery to be consistent
    ORA-01110: data file 1:
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf'
    
    
    ORA-01112: media recovery not started
    
    
    SQL> alter database open resetlogs;
    alter database open resetlogs
    *
    ERROR at line 1:
    ORA-01194: file 1 needs more recovery to be consistent
    ORA-01110: data file 1:
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf'
    


in this case , we don't need "_allow_resetlogs_corruption" anymore :

    $ dsr dbv /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
    auto detected endian is LITTLE ENDIAN
    auto detected block size is 8192
    
    
    block count is 98560
    valid block count is 98560
    invalid block count is 0
    empty block count is 0
    max scn is  0x0000.0x0014c779 (1361785)


max scn in system01.dbf is 1361785, let us set checkpoint scn to 1561785

    SQL> set linesize 300 pagesize 2000
    SQL> select 'dsr ckpscn 1561785 '|| name from v$datafile;
    
    'DSRCKPSCN1561785'||NAME
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_sysaux_ld9rh41t_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_undotbs1_ld9rh429_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_users_ld9rh42y_.dbf
    dsr ckpscn 1561785 /tmp/example.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_t16_lf90hg78_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_t32_lf90l09j_.dbf


run above script generated by SQL:

    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_sysaux_ld9rh41t_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_undotbs1_ld9rh429_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_users_ld9rh42y_.dbf
    dsr ckpscn 1561785 /tmp/example.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_t16_lf90hg78_.dbf
    dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_t32_lf90l09j_.dbf



example output:

    [oracle@ora7 r]$ dsr ckpscn 1561785 /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
    auto detected endian is LITTLE ENDIAN
    auto detected block size is 8192
    
    old bas 1562450 old wrp 0
    updating scn from 0x0000.0x0017d752 (1562450) to 0x0000.0x0017d4b9 (1561785)
    Backup created at absolute Path: /home/oracle/r/backup/o1_mf_system_ld9rh3yb_.dbf_20230821063521
    restore command : dsr restorehdr /home/oracle/r/backup/o1_mf_system_ld9rh3yb_.dbf_20230821063521 /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
    setting fuzz status kcvfhsta @138 from 0x2004 to 0x2000
    backup file backup/o1_mf_system_ld9rh3yb_.dbf_20230821063521 exists , will skip backup again
    
    ========================================> BLOCK SUMMARY <========================================
    
    on disk chkval  is 0x1afe		compute chkval  is 0x1afe		chkval  is ok!
    on disk tailchk is 0x00000b01		compute tailchk is 0x00000b01		tailchk is ok!
    rdba is 0x00400001 ( FILE 1 BLOCK 1 )
    looks like a valid datafile header block
    ckp scn:          1561785 	(0x0000 , 0x0017d4b9)
    rls scn:          1561786 	(0x0000 , 0x0017d4ba)
    crs scn:                7 	(0x0000 , 0x00000007)
    db_name: 		 OLTP
    dbid: 			 1723319014 (0x66b7c2e6)
    type_kcbh:		 0x0b
    frmt_kcbh:		 0xa2
    kccfhswv:		 0x00000000	software format version number
    kccfhcvn:		 0x0b200400	compatibility control
    ts_name: 		 SYSTEM
    TSN: 			 0
    fuzz: 			 0x2000
    rfn: 			 1
    fno: 			 1
    kcvfhcrt		 CREATION TIME: 0x3121c97d ( 1996-02-14 06:37:33 -0500 EST )
    kcvfhrlc: 		 0x4445da03
    kccfhcsq: 		 0x000010ce
    kcvfhccc: 		 0x000000b7
    kcvcptim: 		 0x4445da12 ( 2006-04-19 02:34:58 -0400 EDT )
    kcvfhcpc: 		 0x000000b8
    kccfhfsz		 FILE SIZE: 0x00017700 = 96000 blocks * 8192 = 750 MB = 786432000 bytes
    real size should be 	 786440192 according to the file header
    file real size: 	 786440192 bytes = 750 MB = 96000 blocks * 8192 + 1 * 8192 offset + 0 bytes
    file block count is ok
    ZeroBlock indicates block size is 0x2000 = 8192
    ZeroBlock indicates block count is 0x00017700 = 96000
    
    =============================>random sample 10 data blocks from file<============================
    
        offset:        673669120	rdba:	0x0041413b	file    1	block    82235
        offset:        613384192	rdba:	0x0041247c	file    1	block    74876
        offset:        679313408	rdba:	0x004143ec	file    1	block    82924
        offset:        164225024	rdba:	0x00404e4f	file    1	block    20047
        offset:        286752768	rdba:	0x004088bc	file    1	block    35004
        offset:        225927168	rdba:	0x00406bbb	file    1	block    27579
        offset:        494739456	rdba:	0x0040ebe9	file    1	block    60393
        offset:        342908928	rdba:	0x0040a383	file    1	block    41859
        offset:        404561920	rdba:	0x0040c0e9	file    1	block    49385
        offset:        394993664	rdba:	0x0040bc59	file    1	block    48217




recreate controlfile and open database :

    sqlplus / as sysdba <<EOF
    
    shutdown abort;
    startup nomount;
    alter system set undo_management=manual scope=spfile;
    startup force nomount;
    CREATE CONTROLFILE REUSE DATABASE "OLTP" RESETLOGS  NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
    LOGFILE
    GROUP 1 '/opt/oracle/oradata/OLTP/onlinelog/o1_mf_1_ld9robr2_.log'  SIZE 50M BLOCKSIZE 512,
    GROUP 2 '/opt/oracle/oradata/OLTP/onlinelog/o1_mf_2_ld9rohx6_.log'  SIZE 50M BLOCKSIZE 512,
    GROUP 3 '/opt/oracle/oradata/OLTP/onlinelog/o1_mf_3_ld9ronjn_.log'  SIZE 50M BLOCKSIZE 512
    -- STANDBY LOGFILE
    DATAFILE
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf',
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_sysaux_ld9rh41t_.dbf',
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_undotbs1_ld9rh429_.dbf',
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_users_ld9rh42y_.dbf',
    '/tmp/example.dbf',
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_t16_lf90hg78_.dbf',
    '/opt/oracle/oradata/OLTP/datafile/o1_mf_t32_lf90l09j_.dbf'
    CHARACTER SET US7ASCII
    ;
    
    Control file created.

    
    ALTER DATABASE OPEN RESETLOGS;

    Database altered.

    
    alter tablespace TEMP add tempfile SIZE 500M ;

    Tablespace altered.


    select count(*) from dba_tables;

      COUNT(*)
    ----------
          2904
    
    create table ttt(t1 int);

    *
    ERROR at line 1:
    ORA-00604: error occurred at recursive SQL level 1
    ORA-08102: index key not found, obj# 39, file 1, block 54783 (2)

    
    create undo tablespace UNDOZ1 datafile size 500M;

    Tablespace created.

    alter system set undo_management=AUTO scope=spfile;

    System altered.

    alter system set undo_tablespace=UNDOZ1 scope=spfile;

    System altered.

    shutdown immediate;

    startup;
    
    
    EOF


everytime dsr edits a datafile , it will backup the datafile header to a file named like this:

    old bas 1361398 old wrp 157876224
    updating scn from 0x9690000.0x0014c5f6 (678073218897331702) to 0x0000.0x0017d4b9 (1561785)
    Backup created at absolute Path: /home/oracle/r/backup/o1_mf_t16_lf90hg78_.dbf_20230821063343
    restore command : dsr restorehdr /home/oracle/r/backup/o1_mf_t16_lf90hg78_.dbf_20230821063343 /opt/oracle/oradata/OLTP/datafile/o1_mf_t16_lf90hg78_.dbf




## CASE 2 Oracle Datafile Damaged by Malware/Ransomware -- devos

We need repair the datafile header , and then recover the database.

We will fake a datafile header first , and then change some parameter .


### initfake : initialize a fake datafile header
### rfn      : set rfn for a datafile
### fno      : set fno for a datafile
### tsn      : set TS$.TS# for a datafile
### tsname   : set TS$.NAME for a datafile
### crsscn   : set crsscn for a datafile
### fitsize  : make datafile header fit to the real size
### ckpscn   : set checkpoint scn for a datafile







    export ORACLE_SID=ORCL
    dsr initfake sysaux.Devos
    dsr initfake appdata5.Devos
    dsr initfake appdata4.Devos
    dsr initfake appdata3.Devos
    dsr initfake appdata2.Devos
    dsr initfake appdata1.Devos
    dsr initfake users01.Devos
    dsr initfake undotbs1.Devos
    dsr initfake system.Devos
    
    
    
    dsr fno  1        system.Devos
    dsr rfn  1        system.Devos
    dsr tsn  0        system.Devos
    dsr tsname SYSTEM system.Devos
    dsr crsscn 7      system.Devos
    dsr fitsize       system.Devos
    
    
    
    
    dsr fno  2         sysaux.Devos
    dsr rfn  2         sysaux.Devos
    dsr tsn  1         sysaux.Devos
    dsr tsname SYSAUX  sysaux.Devos
    dsr crsscn 1835       sysaux.Devos
    dsr fitsize        sysaux.Devos
    
    
    
    
    dsr fno  3        undotbs1.Devos
    dsr rfn  3        undotbs1.Devos
    dsr tsn  2        undotbs1.Devos
    dsr tsname UNDOTBS1  undotbs1.Devos
    dsr crsscn 894817      undotbs1.Devos
    dsr fitsize       undotbs1.Devos
    
    
    
    
    
    dsr fno  5        appdata1.Devos
    dsr rfn  5        appdata1.Devos
    dsr tsn  6        appdata1.Devos
    dsr tsname APPDATA appdata1.Devos
    dsr crsscn 942964      appdata1.Devos
    dsr fitsize       appdata1.Devos
    
    
    
    
    dsr fno  6          appdata2.Devos
    dsr rfn  6          appdata2.Devos
    dsr tsn  6          appdata2.Devos
    dsr tsname APPDATA  appdata2.Devos
    dsr crsscn 943286        appdata2.Devos
    dsr fitsize         appdata2.Devos
    
    
    
    
    dsr fno  7        appdata3.Devos
    dsr rfn  7        appdata3.Devos
    dsr tsn  6        appdata3.Devos
    dsr tsname APPDATA appdata3.Devos
    dsr crsscn 943555      appdata3.Devos
    dsr fitsize       appdata3.Devos
    
    
    
    
    dsr fno  8        appdata4.Devos
    dsr rfn  8        appdata4.Devos
    dsr tsn  6        appdata4.Devos
    dsr tsname APPDATA appdata4.Devos
    dsr crsscn 943834     appdata4.Devos
    dsr fitsize       appdata4.Devos
    
    
    
    
    dsr fno  9        appdata5.Devos
    dsr rfn  9        appdata5.Devos
    dsr tsn  6        appdata5.Devos
    dsr tsname APPDATA appdata5.Devos
    dsr crsscn 944272     appdata5.Devos
    dsr fitsize       appdata5.Devos
    
    
    
    
    
    dsr ckpscn 1366772141 sysaux.Devos
    dsr ckpscn 1366772141 appdata5.Devos
    dsr ckpscn 1366772141 appdata4.Devos
    dsr ckpscn 1366772141 appdata3.Devos
    dsr ckpscn 1366772141 appdata2.Devos
    dsr ckpscn 1366772141 appdata1.Devos
    dsr ckpscn 1366772141 users01.Devos
    dsr ckpscn 1366772141 undotbs1.Devos
    dsr ckpscn 1366772141 system.Devos
    
    
    
    
    
    
    export ORACLE_SID=ORCL
    
    sqlplus / as sysdba  <<EOF
    shutdown abort;
    startup nomount;
    create spfile from pfile;
    shutdown abort;
    startup nomount;
    alter system set undo_management=MANUAL scope=spfile;
    shutdown abort;
    
    startup nomount;
    CREATE CONTROLFILE REUSE DATABASE "ORCL"  RESETLOGS  NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
    -- STANDBY LOGFILE
    DATAFILE
    '/d02/devos/sysaux.Devos',
    '/d02/devos/appdata5.Devos',
    '/d02/devos/appdata4.Devos',
    '/d02/devos/appdata3.Devos',
    '/d02/devos/appdata2.Devos',
    '/d02/devos/appdata1.Devos',
    '/d02/devos/undotbs1.Devos',
    '/d02/devos/system.Devos'
    CHARACTER SET ZHS16GBK;
    
    
    
    
    ALTER DATABASE OPEN RESETLOGS;
    
    
    alter tablespace temp add tempfile '/d02/devos/temp01.dbf' size 500M reuse;
    
    
    
    
    EOF
    
    
    exp \'\/ as sysdba\' file=\/dev\/null volsize=2000g owner=HDappdataHQS,HDappdataCTS
    
    
    sqlplus / as sysdba <<EOF
    shutdown abort;
    EOF



## Case 3 Oracle Datafile Damaged by Malware/Ransomware -- eking


    cd /d02/ek/
    rm -rf *.eking
    7za x rs.zip
    
    #dsr mt /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
    
    
    
    
    
    dsr initfake sysaux.eking
    dsr initfake system.eking
    dsr initfake undo.eking
    dsr initfake appdata.eking
    
    
    
    
    
    
    dsr fuzz 0x2000   system.eking
    dsr fno  1        system.eking
    dsr rfn  1        system.eking
    dsr tsn  0        system.eking
    dsr tsname SYSTEM system.eking
    dsr crsscn 7      system.eking
    dsr fitsize       system.eking
    
    
    dsr fuzz 0x0000   sysaux.eking
    dsr fno  2        sysaux.eking
    dsr rfn  2        sysaux.eking
    dsr tsn  1        sysaux.eking
    dsr tsname SYSAUX sysaux.eking
    dsr crsscn 2160   sysaux.eking
    dsr fitsize       sysaux.eking
    
    
    
    
    
    
    dsr fuzz 0x0000      undo.eking
    dsr fno  3           undo.eking
    dsr rfn  3           undo.eking
    dsr tsn  2           undo.eking
    dsr tsname UNDOTBS1  undo.eking
    dsr crsscn 944668    undo.eking
    dsr fitsize          undo.eking
    
    
    
    
    
    dsr fuzz 0x0000       appdata.eking
    dsr fno  5            appdata.eking
    dsr rfn  5            appdata.eking
    dsr tsn  6            appdata.eking
    dsr tsname APPDATA    appdata.eking
    dsr crsscn 1029269    appdata.eking
    dsr fitsize           appdata.eking
    
    
    
    
    dsr ckpscn 19368619  sysaux.eking
    dsr ckpscn 19368619  system.eking
    dsr ckpscn 19368619  undo.eking
    dsr ckpscn 19368619  appdata.eking
    
    
    chmod 777 *.eking
    
    
    sqlplus / as sysdba <<EOF
    shutdown abort;
    startup nomount;
    CREATE CONTROLFILE REUSE DATABASE "ORCL"  RESETLOGS  NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
    -- STANDBY LOGFILE
    DATAFILE
    '/d02/ek/system.eking',
    '/d02/ek/sysaux.eking',
    '/d02/ek/undo.eking',
    '/d02/ek/appdata.eking'
    CHARACTER SET ZHS16GBK
    ;
    
    
    ALTER DATABASE OPEN RESETLOGS;
    
    
    alter tablespace temp add tempfile '/d02/ek/temp01.dbf' size 500M reuse;
    
    EOF
    
    
    
    exp \'\/ as sysdba\' file=\/dev\/null volsize=2000g owner=appdataS
    
    
    sqlplus / as sysdba <<EOF
    shutdown abort;
    EOF
