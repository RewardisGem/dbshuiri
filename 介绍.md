

# DBSHUIRI 

# 介绍 DBSHUIRI是一个快速修复损坏ORACLE的工具。它可以针对多种损坏场景，快速修复ORACLE数据文件，使得ORACLE可以启动。


    $ dsr
    dbshuri tool for quick fix oracle datafile Release 2309
    Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
    Free for personal use; commercial users need a license.
    
    Usage:
    dsr [command]
    
    Available Commands:
    backuphdr   backup datafile header
    ckpscn      set hdr checkpoint scn
    cleanup     cleanup sqlite db
    completion  Generate the autocompletion script for the specified shell
    copy        copy block
    copyany     copyany block
    crsscn      set hdr creation scn
    crt         set hdr crt
    csq         set hdr csq
    cvn         set hdr cvn
    damage1     damage a file with random block
    damage2     damage a file with first 0.25MB and last 1MB
    dbi         set hdr dbid
    dbname      set hdr database name
    dbv         check a datafile
    dumpexp     dump exp file
    dumppump    dump expdp file
    dumpsys     dump create database sql & dsr script
    fill        fill a datafile with size
    fitsize     set hdr with a fit size
    fno         set hdr file#
    fsz         set hdr fsz
    fuzz        set hdr fuzz status
    guess       guess block size
    hdr         show datafile header content
    help        Help about any command
    hxd         hexdump a file
    hxdb        hexdump a 8192 bytes block
    initfake    initfake hdr
    makerdba    make rdba
    markcorrupt mark a block as corrupt
    mt          make template
    parsenum    parse number
    parserdba   parse rdba
    query       query in sqlite db
    querydbf    querydbf datafile from sqlite
    rdba        set hdr rdba
    register    register license key
    restoredbf  restore datafile from disk image
    restorehdr  restore datafile header from backup
    rfn         set hdr rfile#
    rlc         set hdr resetlogs counter
    rlsscn      set hdr resetlogs
    rt          read template
    scandev     scan device
    sum         calc sum for a block
    sumapply    sum apply a block
    test        test
    tsn         set hdr tsn
    tsname      set hdr tablespace name
    version     print version info
    wt          write template
    
    Flags:
    --blocksize int   The BLOCKSIZE can be 8192,16384,32768.
    --endian string   The ENDIAN can be BIG,LITTLE.
    -h, --help            help for dsr
    
    Use "dsr [command] --help" for more information about a command.
    [oracle@ora7 ~]$ dsr ckpscn
    Error: accepts 2 arg(s), received 0
    Usage:
    dsr ckpscn [flags]
    
    Examples:
    dsr ckpscn 1234567890 users01.dbf
    
    Flags:
    -h, --help   help for ckpscn
    
    Global Flags:
    --blocksize int   The BLOCKSIZE can be 8192,16384,32768.
    --endian string   The ENDIAN can be BIG,LITTLE.




## ORACLE当前日志损坏，无法启动。可以使用dsr ckpscn <scn> users01.dbf命令，将datafile header的检查点SCN修复为指定的SCN，并清除FUZZY状态，使得ORACLE可以启动。


### dsr hdr <datafile> 查看kcvfh数据文件头信息

      $ dsr hdr ORA_1.dbf
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      auto detected endian is LITTLE ENDIAN
      auto detected block size is 8192
      
      
      ========================================> BLOCK SUMMARY <========================================
      
      on disk chkval  is 0xb2e1		compute chkval  is 0xb2e1		chkval  is ok!
      on disk tailchk is 0x00000b01		compute tailchk is 0x00000b01		tailchk is ok!
      rdba is 0x00400001 ( FILE 1 BLOCK 1 )
      looks like a valid datafile header block
      ckp scn:          2823558 	(0x0000 , 0x8000 , 0x002b1586)
      rls scn:          1920977 	(0x0000 , 0x8000 , 0x001d4fd1)
      crs scn:                9 	(0x0000 , 0x8000 , 0x00000009)
      db_name: 		 OO19C
      dbid: 			 3027246059 (0xb4701beb)
      type_kcbh:		 0x0b
      frmt_kcbh:		 0xa2
      kccfhswv:		 0x00000000	software format version number
      kccfhcvn:		 0x13000000	compatibility control
      ts_name: 		 SYSTEM
      TSN: 			 0
      fuzz: 			 0x2000
      rfn: 			 1
      fno: 			 1
      kcvfhcrt		 CREATION TIME: 0x3bf312a9 ( 2001-11-14 19:56:09 -0500 EST )
      kcvfhrlc: 		 0x444c562d
      kccfhcsq: 		 0x000033e9
      kcvfhccc: 		 0x000000df
      kcvcptim: 		 0x445c0a08 ( 2006-05-05 22:29:28 -0400 EDT )
      kcvfhcpc: 		 0x000000e0
      kccfhfsz		 FILE SIZE: 0x0001d100 = 119040 blocks * 8192 = 930 MB = 975175680 bytes
      real size should be 	 975183872 according to the file header
      file real size: 	 975183872 bytes = 930 MB = 119040 blocks * 8192 + 1 * 8192 offset + 0 bytes
      file block count is ok
      ZeroBlock indicates block size is 0x2000 = 8192
      ZeroBlock indicates block count is 0x0001d100 = 119040
      
      =============================>random sample 10 data blocks from file<============================
      
          offset:        355663872	rdba:	0x0040a998	file    1	block    43416
          offset:        345432064	rdba:	0x0040a4b7	file    1	block    42167
          offset:        825147392	rdba:	0x00418976	file    1	block   100726
          offset:        483426304	rdba:	0x0040e684	file    1	block    59012
          offset:        476659712	rdba:	0x0040e34a	file    1	block    58186
          offset:        897843200	rdba:	0x0041ac20	file    1	block   109600
          offset:        835944448	rdba:	0x00418e9c	file    1	block   102044
          offset:         94568448	rdba:	0x00402d18	file    1	block    11544
          offset:        782131200	rdba:	0x004174f3	file    1	block    95475
          offset:        719142912	rdba:	0x004156ea	file    1	block    87786



### CASE Lost Current Redo Log File


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


## ORACLE数据文件头损坏，例如被计算机病毒破坏或加密。可以使用如下的系列命令伪造一个有效的数据文件头，使得ORACLE可以启动。


        dsr initfake      system.Devos
        dsr fno  1        system.Devos
        dsr rfn  1        system.Devos
        dsr tsn  0        system.Devos
        dsr tsname SYSTEM system.Devos
        dsr crsscn 7      system.Devos
        dsr fitsize       system.Devos


### CASE Oracle Datafile Damaged by Malware/Ransomware -- devos

We need repair the datafile header , and then recover the database.

We will fake a datafile header first , and then change some parameter .


initfake : initialize a fake datafile header
rfn      : set rfn for a datafile
fno      : set fno for a datafile
tsn      : set TS$.TS# for a datafile
tsname   : set TS$.NAME for a datafile
crsscn   : set crsscn for a datafile
fitsize  : make datafile header fit to the real size
ckpscn   : set checkpoint scn for a datafile







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



### CASE Oracle Datafile Damaged by Malware/Ransomware -- eking


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


&nbsp;
## 对于控制文件损坏的情况，可以通过分析SYSTEM01.DBF来获得重建控制文件的SQL语句。例如：
 
&nbsp;
&nbsp;





        $ dsr dumpsys /d02/19c/system01.dbf

        DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
        Free for personal use; commercial users need a license.

        auto detected endian is LITTLE ENDIAN
        auto detected block size is 8192
        
        Please confirm the input file is system01.dbf, otherwise it won't work
        
            TSNO#: 0	NAME: SYSTEM	BLOCKSIZE: 8192
            TSNO#: 1	NAME: SYSAUX	BLOCKSIZE: 8192
            TSNO#: 2	NAME: UNDOTBS1	BLOCKSIZE: 8192
            TSNO#: 3	NAME: TEMP	BLOCKSIZE: 8192
            TSNO#: 4	NAME: USERS	BLOCKSIZE: 8192
            TSNO#: 5	NAME: UNDOTBS2	BLOCKSIZE: 8192
        FILE#: 1 STATUS$: 2 BLOCKS: 64000 TSNO#: 0 RELFILE#: 1 CrscnWrp: 0 CrscnBas: 9
        FILE#: 3 STATUS$: 2 BLOCKS: 51200 TSNO#: 1 RELFILE#: 3 CrscnWrp: 0 CrscnBas: 5480
        FILE#: 5 STATUS$: 1 BLOCKS: 3200 TSNO#: 0 RELFILE#: 0 CrscnWrp: 4194302 CrscnBas: 1280
        FILE#: 7 STATUS$: 2 BLOCKS: 640 TSNO#: 4 RELFILE#: 7 CrscnWrp: 0 CrscnBas: 32876
        FILE#: 2 STATUS$: 1 BLOCKS: 256 TSNO#: 0 RELFILE#: 0 CrscnWrp: 0 CrscnBas: 0
        FILE#: 4 STATUS$: 2 BLOCKS: 3200 TSNO#: 2 RELFILE#: 4 CrscnWrp: 0 CrscnBas: 1920446
        ...........
        
        FILE#: 1 STATUS$: 2 BLOCKS: 64000 TS#: 0 RELFILE#: 1 CrscnWrp: 0 CrscnBas: 9 TS_NAME: SYSTEM FILENAME: SYSTEM_0_1.dbf BLOCKSIZE: 8192
        FILE#: 3 STATUS$: 2 BLOCKS: 51200 TS#: 1 RELFILE#: 3 CrscnWrp: 0 CrscnBas: 5480 TS_NAME: SYSAUX FILENAME: SYSAUX_1_3.dbf BLOCKSIZE: 8192
        FILE#: 5 STATUS$: 1 BLOCKS: 3200 TS#: 0 RELFILE#: 0 CrscnWrp: 4194302 CrscnBas: 1280 TS_NAME: SYSTEM FILENAME: SYSTEM_0_0.dbf BLOCKSIZE: 8192
        FILE#: 7 STATUS$: 2 BLOCKS: 640 TS#: 4 RELFILE#: 7 CrscnWrp: 0 CrscnBas: 32876 TS_NAME: USERS FILENAME: USERS_4_7.dbf BLOCKSIZE: 8192
        FILE#: 2 STATUS$: 1 BLOCKS: 256 TS#: 0 RELFILE#: 0 CrscnWrp: 0 CrscnBas: 0 TS_NAME: SYSTEM FILENAME: SYSTEM_0_0.dbf BLOCKSIZE: 8192
        FILE#: 4 STATUS$: 2 BLOCKS: 3200 TS#: 2 RELFILE#: 4 CrscnWrp: 0 CrscnBas: 1920446 TS_NAME: UNDOTBS1 FILENAME: UNDOTBS1_2_4.dbf BLOCKSIZE: 8192
        
        
        DB NAME : OO19C
        NLS_RDBMS_VERSION : 19.0.0.0.0
        NLS_CHARACTERSET : AL32UTF8
        NLS_NCHAR_CHARACTERSET : AL16UTF16
        
        ==================================>   DSR SCRIPT  <====================================
        
        
        dsr initfake SYSTEM_0_1.dbf
        dsr cvn 0x13000000 SYSTEM_0_1.dbf
        dsr dbname OO19C SYSTEM_0_1.dbf
        dsr tsname SYSTEM SYSTEM_0_1.dbf
        dsr tsn 0 SYSTEM_0_1.dbf
        dsr fno 1 SYSTEM_0_1.dbf
        dsr rfn 1 SYSTEM_0_1.dbf
        dsr crsscn 9 SYSTEM_0_1.dbf
        dsr fitsize  SYSTEM_0_1.dbf
        dsr ckpscn 12100160 SYSTEM_0_1.dbf
        #get maxScn from scanning datafile 2100160 plus 10000000
        
        
        
        
        dsr initfake SYSAUX_1_3.dbf
        dsr cvn 0x13000000 SYSAUX_1_3.dbf
        dsr dbname OO19C SYSAUX_1_3.dbf
        dsr tsname SYSAUX SYSAUX_1_3.dbf
        dsr tsn 1 SYSAUX_1_3.dbf
        dsr fno 3 SYSAUX_1_3.dbf
        dsr rfn 3 SYSAUX_1_3.dbf
        dsr crsscn 5480 SYSAUX_1_3.dbf
        dsr fitsize  SYSAUX_1_3.dbf
        dsr ckpscn 12100160 SYSAUX_1_3.dbf
        #get maxScn from scanning datafile 2100160 plus 10000000
        
        
        
        
        dsr initfake SYSTEM_0_0.dbf
        dsr cvn 0x13000000 SYSTEM_0_0.dbf
        dsr dbname OO19C SYSTEM_0_0.dbf
        dsr tsname SYSTEM SYSTEM_0_0.dbf
        dsr tsn 0 SYSTEM_0_0.dbf
        dsr fno 5 SYSTEM_0_0.dbf
        dsr rfn 0 SYSTEM_0_0.dbf
        dsr crsscn 18014389919548672 SYSTEM_0_0.dbf
        dsr fitsize  SYSTEM_0_0.dbf
        dsr ckpscn 3366772141 SYSTEM_0_0.dbf
        #can't get valid maxscn from datafile , now using 1366772141,you need to replace it
        
        
        
        
        dsr initfake USERS_4_7.dbf
        dsr cvn 0x13000000 USERS_4_7.dbf
        dsr dbname OO19C USERS_4_7.dbf
        dsr tsname USERS USERS_4_7.dbf
        dsr tsn 4 USERS_4_7.dbf
        dsr fno 7 USERS_4_7.dbf
        dsr rfn 7 USERS_4_7.dbf
        dsr crsscn 32876 USERS_4_7.dbf
        dsr fitsize  USERS_4_7.dbf
        dsr ckpscn 12100160 USERS_4_7.dbf
        #get maxScn from scanning datafile 2100160 plus 10000000
        
        
        
        
        dsr initfake SYSTEM_0_0.dbf
        dsr cvn 0x13000000 SYSTEM_0_0.dbf
        dsr dbname OO19C SYSTEM_0_0.dbf
        dsr tsname SYSTEM SYSTEM_0_0.dbf
        dsr tsn 0 SYSTEM_0_0.dbf
        dsr fno 2 SYSTEM_0_0.dbf
        dsr rfn 0 SYSTEM_0_0.dbf
        dsr crsscn 0 SYSTEM_0_0.dbf
        dsr fitsize  SYSTEM_0_0.dbf
        dsr ckpscn 12100160 SYSTEM_0_0.dbf
        #get maxScn from scanning datafile 2100160 plus 10000000
        
        
        
        
        dsr initfake UNDOTBS1_2_4.dbf
        dsr cvn 0x13000000 UNDOTBS1_2_4.dbf
        dsr dbname OO19C UNDOTBS1_2_4.dbf
        dsr tsname UNDOTBS1 UNDOTBS1_2_4.dbf
        dsr tsn 2 UNDOTBS1_2_4.dbf
        dsr fno 4 UNDOTBS1_2_4.dbf
        dsr rfn 4 UNDOTBS1_2_4.dbf
        dsr crsscn 1920446 UNDOTBS1_2_4.dbf
        dsr fitsize  UNDOTBS1_2_4.dbf
        dsr ckpscn 12100160 UNDOTBS1_2_4.dbf
        #get maxScn from scanning datafile 2100160 plus 10000000
        
        
        
        ==============================> Create Control File SQL <==============================
        
        CREATE CONTROLFILE REUSE DATABASE "OO19C" RESETLOGS  NOARCHIVELOG
        MAXLOGFILES 200
        MAXLOGMEMBERS 5
        MAXDATAFILES 1000
        MAXINSTANCES 8
        MAXLOGHISTORY 1000
        --LOGFILE  GROUP 1 '/d01/oradata/redo01.log' SIZE 500M, GROUP 2 '/d01/oradata/redo02.log' SIZE 500M,GROUP 3 '/d01/oradata/redo03.log' SIZE 500M
        DATAFILE
        'SYSTEM_0_1.dbf',
        'SYSAUX_1_3.dbf',
        'SYSTEM_0_0.dbf',
        'USERS_4_7.dbf',
        'SYSTEM_0_0.dbf',
        'UNDOTBS1_2_4.dbf'
        CHARACTER SET AL32UTF8;


以上通过dumpsys命令分析SYSTEM01.DBF，生成了重建控制文件的SQL语句和DSR伪造所有数据文件头的命令。


## 对于损坏的ORACLE EXP或EXPDP导出文件，可以使用dsr dumpexp或dsr dumppump命令分析导出文件，生成导出文件的SQL语句。例如：

    $ dsr dumpexp sh.exp
        


    ==================================================>END   table "SALES" OFFSET 15027609 ~ 15027609 = 0 B  processed 0 rows<==================================================
    
    TABLE SALES output file path : /home/oracle/data_1694581378/SALES_15027609.csv
    
    
    ==================================================>END   table "PRODUCTS" OFFSET 14930167 ~ 14944273 = 13 KB  processed 72 rows<==================================================
    
    TABLE PRODUCTS output file path : /home/oracle/data_1694581378/PRODUCTS_14930167.csv
    
    
    ==================================================>END   table "PROMOTIONS" OFFSET 14957390 ~ 15012840 = 54 KB  processed 503 rows<==================================================
    
    TABLE PROMOTIONS output file path : /home/oracle/data_1694581378/PROMOTIONS_14957390.csv
    
    
    ==================================================>END   table "SUPPLEMENTARY_DEMOGRAPHICS" OFFSET 50800846 ~ 51558135 = 739 KB  processed 4500 rows<==================================================
    
    TABLE SUPPLEMENTARY_DEMOGRAPHICS output file path : /home/oracle/data_1694581378/SUPPLEMENTARY_DEMOGRAPHICS_50800846.csv
    
    
    ==================================================>END   table "TIMES" OFFSET 51567683 ~ 52002222 = 424 KB  processed 1826 rows<==================================================
    
    TABLE TIMES output file path : /home/oracle/data_1694581378/TIMES_51567683.csv
    
    
    ==================================================>END   table "FWEEK_PSCAT_SALES_MV" OFFSET 14469105 ~ 14926031 = 446 KB  processed 11266 rows<==================================================
    
    TABLE FWEEK_PSCAT_SALES_MV output file path : /home/oracle/data_1694581378/FWEEK_PSCAT_SALES_MV_14469105.csv
    
    
    ==================================================>END   table "CUSTOMERS" OFFSET 2966461 ~ 14449251 = 10 MB  processed 55500 rows<==================================================
    
    TABLE CUSTOMERS output file path : /home/oracle/data_1694581378/CUSTOMERS_2966461.csv


    $ dsr dumppump  sh.expdp 

    ==================================================>BEGIN table PRODUCTS OFFSET 46130976 ~ 46143642 = 12 KB <==================================================
    
    
    
    ==================================================>BEGIN table SALES OFFSET 36449768 ~ 38100659 = 1 MB <==================================================
    
    
    
    ==================================================>BEGIN table SALES OFFSET 16932328 ~ 19034900 = 2 MB <==================================================
    
    
    
    ==================================================>BEGIN table SALES OFFSET 38108648 ~ 40213549 = 2 MB <==================================================
    
    
    
    ==================================================>BEGIN table SALES OFFSET 19045864 ~ 21098961 = 1 MB <==================================================
    
    
    
    ==================================================>END   table PRODUCTS OFFSET 46130976 ~ 46143642 = 12 KB  processed 72 rows<==================================================
    
    TABLE PRODUCTS output file path : /home/oracle/pump_1694641065/SH.PRODUCTS_88065.csv
    
    
    ==================================================>BEGIN table CUSTOMERS OFFSET 272384 ~ 10589674 = 9 MB <==================================================
    
    
    
    ==================================================>END   table SALES OFFSET 36449768 ~ 38100659 = 1 MB  processed 48874 rows<==================================================
    
    TABLE SALES output file path : /home/oracle/pump_1694641065/SH.SALES_87995.csv



## 对于损坏的文件系统或ASM，或者误删除的ORACLE数据文件。可以使用dsr scandev <device>命令扫描设备，以合并数据块碎片的原理恢复数据文件。例如：

         $ dsr scandev /dev/sdd1 8192

         DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
         Free for personal use; commercial users need a license.

         Table block_map created successfully.
         HDR block found at offset: 142614528 , DBID: 3027246059 , DBNAME: OO19C , TSNAME: SYSAUX , TSNUM: 1  CKPSCN: 2823558 , RLSSCN: 1920977 , CRSSCN: 5480 , FNO: 3 , RFN: 3 , CVN: 0x13000000 , FSZ: 117760
         Extent found from RDBA 4290772992 ( 1023 , 0 )  with size 8 KB , starting at offset 142606336
         Extent found from RDBA 12582913 ( 3 , 1 ) to 12614655 ( 3 , 31743 ) with size 247 MB , starting at offset 142614528
         Extent found from RDBA 12614656 ( 3 , 31744 ) to 12624895 ( 3 , 41983 ) with size 80 MB , starting at offset 411041792
         HDR block found at offset: 494936064 , DBID: 3027246059 , DBNAME: OO19C , TSNAME: SYSTEM , TSNUM: 0  CKPSCN: 2823558 , RLSSCN: 1920977 , CRSSCN: 9 , FNO: 1 , RFN: 1 , CVN: 0x13000000 , FSZ: 119040
         Extent found from RDBA 4290772992 ( 1023 , 0 )  with size 8 KB , starting at offset 494927872
         Extent found from RDBA 4194305 ( 1 , 1 ) to 4215807 ( 1 , 21503 ) with size 167 MB , starting at offset 494936064
         Extent found from RDBA 4215808 ( 1 , 21504 ) to 4236287 ( 1 , 41983 ) with size 160 MB , starting at offset 679477248
         Extent found from RDBA 12624896 ( 3 , 41984 ) to 12636159 ( 3 , 53247 ) with size 88 MB , starting at offset 847249408
         Extent found from RDBA 12636160 ( 3 , 53248 ) to 12666879 ( 3 , 83967 ) with size 240 MB , starting at offset 947912704
         Extent found from RDBA 4236288 ( 1 , 41984 ) to 4237311 ( 1 , 43007 ) with size 8 MB , starting at offset 1199570944
         Extent found from RDBA 4237312 ( 1 , 43008 ) to 4250623 ( 1 , 56319 ) with size 104 MB , starting at offset 1216348160
         Extent found from RDBA 12666880 ( 3 , 83968 ) to 12700672 ( 3 , 117760 ) with size 264 MB , starting at offset 1325400064
         Extent found from RDBA 4250624 ( 1 , 56320 ) to 4292607 ( 1 , 98303 ) with size 328 MB , starting at offset 1610612736
         HDR block found at offset: 1954553856 , DBID: 3027246059 , DBNAME: OO19C , TSNAME: UNDOTBS1 , TSNUM: 2  CKPSCN: 2823558 , RLSSCN: 1920977 , CRSSCN: 1920446 , FNO: 4 , RFN: 4 , CVN: 0x13000000 , FSZ: 43520
         Extent found from RDBA 4290772992 ( 1023 , 0 )  with size 8 KB , starting at offset 1954545664
         Extent found from RDBA 16777217 ( 4 , 1 ) to 16800767 ( 4 , 23551 ) with size 183 MB , starting at offset 1954553856
         Extent found from RDBA 16800768 ( 4 , 23552 ) to 16819199 ( 4 , 41983 ) with size 144 MB , starting at offset 2281701376
         HDR block found at offset: 2432704512 , DBID: 3027246059 , DBNAME: OO19C , TSNAME: USERS , TSNUM: 4  CKPSCN: 2823558 , RLSSCN: 1920977 , CRSSCN: 32876 , FNO: 7 , RFN: 7 , CVN: 0x13000000 , FSZ: 314240
         Extent found from RDBA 4290772992 ( 1023 , 0 )  with size 8 KB , starting at offset 2432696320
         Extent found from RDBA 29360129 ( 7 , 1 ) to 29402111 ( 7 , 41983 ) with size 327 MB , starting at offset 2432704512
         Extent found from RDBA 4292608 ( 1 , 98304 ) to 4313344 ( 1 , 119040 ) with size 162 MB , starting at offset 2776629248
         Extent found from RDBA 16819200 ( 4 , 41984 ) to 16820736 ( 4 , 43520 ) with size 12 MB , starting at offset 2952790016
         Extent found from RDBA 29402112 ( 7 , 41984 ) to 29449215 ( 7 , 89087 ) with size 368 MB , starting at offset 2969567232
         Extent found from RDBA 29449216 ( 7 , 89088 ) to 29480959 ( 7 , 120831 ) with size 248 MB , starting at offset 3363831808
         Extent found from RDBA 29480960 ( 7 , 120832 ) to 29561855 ( 7 , 201727 ) with size 632 MB , starting at offset 3632267264
         Extent found from RDBA 29561856 ( 7 , 201728 ) to 29649407 ( 7 , 289279 ) with size 684 MB , starting at offset 4429185024
         
         ID | DEVICE_PATH |   OFFSET   |    RDBA    | BLOCKSIZE | FILE_ID | BLOCK_ID | BLOCKS
         ----+-------------+------------+------------+-----------+---------+----------+--------
         1 | /dev/sdd1   |  142606336 | 4290772992 |      8192 |    1023 |        0 |      1
         2 | /dev/sdd1   |  142614528 | 12582913   |      8192 |       3 |        1 |  31743
         3 | /dev/sdd1   |  411041792 | 12614656   |      8192 |       3 |    31744 |  10240
         4 | /dev/sdd1   |  494927872 | 4290772992 |      8192 |    1023 |        0 |      1
         5 | /dev/sdd1   |  494936064 | 4194305    |      8192 |       1 |        1 |  21503
         6 | /dev/sdd1   |  679477248 | 4215808    |      8192 |       1 |    21504 |  20480
         7 | /dev/sdd1   |  847249408 | 12624896   |      8192 |       3 |    41984 |  11264
         8 | /dev/sdd1   |  947912704 | 12636160   |      8192 |       3 |    53248 |  30720
         9 | /dev/sdd1   | 1199570944 | 4236288    |      8192 |       1 |    41984 |   1024
         10 | /dev/sdd1   | 1216348160 | 4237312    |      8192 |       1 |    43008 |  13312
         11 | /dev/sdd1   | 1325400064 | 12666880   |      8192 |       3 |    83968 |  33793
         12 | /dev/sdd1   | 1610612736 | 4250624    |      8192 |       1 |    56320 |  41984
         13 | /dev/sdd1   | 1954545664 | 4290772992 |      8192 |    1023 |        0 |      1
         14 | /dev/sdd1   | 1954553856 | 16777217   |      8192 |       4 |        1 |  23551
         15 | /dev/sdd1   | 2281701376 | 16800768   |      8192 |       4 |    23552 |  18432
         16 | /dev/sdd1   | 2432696320 | 4290772992 |      8192 |    1023 |        0 |      1
         17 | /dev/sdd1   | 2432704512 | 29360129   |      8192 |       7 |        1 |  41983
         18 | /dev/sdd1   | 2776629248 | 4292608    |      8192 |       1 |    98304 |  20737
         19 | /dev/sdd1   | 2952790016 | 16819200   |      8192 |       4 |    41984 |   1537
         20 | /dev/sdd1   | 2969567232 | 29402112   |      8192 |       7 |    41984 |  47104
         21 | /dev/sdd1   | 3363831808 | 29449216   |      8192 |       7 |    89088 |  31744
         22 | /dev/sdd1   | 3632267264 | 29480960   |      8192 |       7 |   120832 |  80896
         23 | /dev/sdd1   | 4429185024 | 29561856   |      8192 |       7 |   201728 |  87552
         (23 rows)
         
         ID | DEVICE_PATH |   OFFSET   | FNO | RFN |    DBID    |  DBNAME  |             TSNAME             | TSNUM | BLOCKSIZE | CKPSCN  | RLSSCN  | CRSSCN  |    CVN    |  FSZ   
         ----+-------------+------------+-----+-----+------------+----------+--------------------------------+-------+-----------+---------+---------+---------+-----------+--------
         1 | /dev/sdd1   |  142614528 |   3 |   3 | 3027246059 | OO19C | SYSAUX |     1 |      8192 | 2823558 | 1920977 |    5480 | 318767104 | 117760
         2 | /dev/sdd1   |  494936064 |   1 |   1 | 3027246059 | OO19C | SYSTEM |     0 |      8192 | 2823558 | 1920977 |       9 | 318767104 | 119040
         3 | /dev/sdd1   | 1954553856 |   4 |   4 | 3027246059 | OO19C | UNDOTBS1 |     2 |      8192 | 2823558 | 1920977 | 1920446 | 318767104 |  43520
         4 | /dev/sdd1   | 2432704512 |   7 |   7 | 3027246059 | OO19C | USERS |     4 |      8192 | 2823558 | 1920977 |   32876 | 318767104 | 314240


         $dsr restoredbf 1

         DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
         Free for personal use; commercial users need a license.
         now write 8k zero block fake header
         auto detected endian is LITTLE ENDIAN
         auto detected block size is 8192
         
         setting kccfhfsz_kcvfhhdr from 119040 to 119040
         Backup created at absolute Path: /var/lib/mysql/backup/ORA_1.dbf_20230911035946
         restore command : dsr restorehdr /var/lib/mysql/backup/ORA_1.dbf_20230911035946 /var/lib/mysql/ORA_1.dbf
         backup file backup/ORA_1.dbf_20230911035946 exists , will skip backup again
         Output written to ORA_1.dbf

         $dbv file=ORA_1.dbf
         
         DBVERIFY: Release 19.0.0.0.0 - Production on Mon Sep 11 04:00:20 2023
         
         Copyright (c) 1982, 2019, Oracle and/or its affiliates.  All rights reserved.
         
         DBVERIFY - Verification starting : FILE = /var/lib/mysql/ORA_1.dbf
         
         
         DBVERIFY - Verification complete
         
         Total Pages Examined         : 119040
         Total Pages Processed (Data) : 83178
         Total Pages Failing   (Data) : 0
         Total Pages Processed (Index): 13589
         Total Pages Failing   (Index): 0
         Total Pages Processed (Other): 5214
         Total Pages Processed (Seg)  : 1
         Total Pages Failing   (Seg)  : 0
         Total Pages Empty            : 17059
         Total Pages Marked Corrupt   : 0
         Total Pages Influx           : 0
         Total Pages Encrypted        : 0
         Highest block SCN            : 2823553 (0.2823553)


POC:

         source /home/oracle/19csh
         rm -rf /home/oracle/d04/ORA*.dbf
         sudo umount -f /d03
         sudo dd if=/dev/zero of=/dev/sdd1 bs=1024M count=32
         sudo mkfs.ext4 /dev/sdd1
         sudo mount /dev/sdd1 /d03
         sudo chown oracle:oinstall /d03
         sudo chown oracle:oinstall /dev/sdd1
         ls -ld /d03
         cd /d03
         7za x /d02/bak/pd-db.7z
         rm -rf /d03/*.dbf
         mkdir /home/oracle/d04
         cd /home/oracle/d04
         sudo umount -f /d03
         
         
         
         dsr cleanup
         dsr scandev /dev/sdd1 8192
         
         dsr restoredbf 1
         dsr restoredbf 3
         dsr restoredbf 4   
         dsr restoredbf 7
         
         
         rman target / <<EOF
         startup mount
         catalog datafilecopy '/home/oracle/d04/ORA_1.dbf';
         catalog datafilecopy '/home/oracle/d04/ORA_3.dbf';
         catalog datafilecopy '/home/oracle/d04/ORA_4.dbf';
         catalog datafilecopy '/home/oracle/d04/ORA_7.dbf';
         
         switch database to copy;
         alter database open read only;
         report schema;
         EOF
         
         exp \'\/ as sysdba\' file=\/dev\/null volsize=2000g owner=pd1




## DSR可以对数据文件做简单的检测，例如：

         $ dsr dbv ORA_1.dbf
         DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
         Free for personal use; commercial users need a license.
         auto detected endian is LITTLE ENDIAN
         auto detected block size is 8192
         
         
         block count is 119040
         valid block count is 119040
         invalid block count is 0
         empty block count is 0
         max scn is  0x0000.0x002b1581 (2823553)

不同于ORACLE自带的DBV工具，即便数据文件头部损坏，仍可以使用dsr dbv对剩余数据块做检测


## 一些辅助功能：

### dsr guess <datafile>   猜测数据文件的块大小和ENDIAN

      $ dsr guess ORA_1.dbf

      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.

      Free for personal use; commercial users need a license.

      We guess the endian is : 	LITTLE ENDIAN
      We guess the block size is : 	8192 bytes


### dsr copy <source.dbf> <target.dbf> 10 复制source.dbf的第10个数据块到target.dbf的第10个数据块

&nbsp;
&nbsp;
### dsr backuphdr users01.dbf  备份数据文件头
&nbsp;
&nbsp;

      $ dsr backuphdr o1_mf_system_ld9rh3yb_.dbf
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      auto detected endian is LITTLE ENDIAN
      auto detected block size is 8192
      
      Backup created at absolute Path: /opt/oracle/oradata/OLTP/datafile/backup/o1_mf_system_ld9rh3yb_.dbf_20230911042051
      restore command : dsr restorehdr /opt/oracle/oradata/OLTP/datafile/backup/o1_mf_system_ld9rh3yb_.dbf_20230911042051 /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf


&nbsp;
### dsr restorehdr <backupfile> <target.dbf>  从备份恢复数据文件头
&nbsp;

      $ dsr restorehdr /opt/oracle/oradata/OLTP/datafile/backup/o1_mf_system_ld9rh3yb_.dbf_20230911042051 /opt/oracle/oradata/OLTP/datafile/o1_mf_system_ld9rh3yb_.dbf
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      auto detected endian is LITTLE ENDIAN
      auto detected block size is 8192
      
      
      ========================================> BLOCK SUMMARY <========================================
      
      on disk chkval  is 0x9c61		compute chkval  is 0x9c61		chkval  is ok!
      on disk tailchk is 0x00000b01		compute tailchk is 0x00000b01		tailchk is ok!
      rdba is 0x00400001 ( FILE 1 BLOCK 1 )
      looks like a valid datafile header block
      ckp scn:          2113568 	(0x0969 , 0x0000 , 0x00204020)
      rls scn:           925702 	(0x0000 , 0x0000 , 0x000e2006)
      crs scn:                7 	(0x0000 , 0x0000 , 0x00000007)
      db_name: 		 OLTP
      dbid: 			 1723319014 (0x66b7c2e6)
      type_kcbh:		 0x0b
      frmt_kcbh:		 0xa2
      kccfhswv:		 0x00000000	software format version number
      kccfhcvn:		 0x0b200400	compatibility control
      ts_name: 		 SYSTEM
      TSN: 			 0
      fuzz: 			 0x2004
      rfn: 			 1
      fno: 			 1
      kcvfhcrt		 CREATION TIME: 0x3121c97d ( 1996-02-14 06:37:33 -0500 EST )
      kcvfhrlc: 		 0x44277ee9
      kccfhcsq: 		 0x00002b98
      kcvfhccc: 		 0x000001e6
      kcvcptim: 		 0x4460fbc1 ( 2006-05-09 16:29:53 -0400 EDT )
      kcvfhcpc: 		 0x000001e7
      kccfhfsz		 FILE SIZE: 0x000d9d00 = 892160 blocks * 8192 = 6 GB = 7308574720 bytes
      real size should be 	 7308582912 according to the file header
      file real size: 	 7308582912 bytes = 6 GB = 892160 blocks * 8192 + 1 * 8192 offset + 0 bytes
      file block count is ok
      ZeroBlock indicates block size is 0x2000 = 8192
      ZeroBlock indicates block count is 0x000d9d00 = 892160
      
      =============================>random sample 10 data blocks from file<============================
      
          offset:       1180467200	rdba:	0x004232e4	file    1	block   144100
          offset:       1475010560	rdba:	0x0042bf57	file    1	block   180055
          offset:       5565259776	rdba:	0x004a5db9	file    1	block   679353
          offset:       4355964928	rdba:	0x00481d16	file    1	block   531734
          offset:       2796806144	rdba:	0x0045359f	file    1	block   341407
          offset:       4558143488	rdba:	0x00487d7e	file    1	block   556414
          offset:       3338256384	rdba:	0x004637ce	file    1	block   407502
          offset:       1719975936	rdba:	0x00433426	file    1	block   209958
          offset:       6164725760	rdba:	0x004b7b92	file    1	block   752530
          offset:        629424128	rdba:	0x00412c22	file    1	block    76834

&nbsp;
&nbsp;
### dsr parserdba 0x00400001 解析RDBA到RFILE# BLOCK#
&nbsp;
&nbsp;


      $ dsr parserdba  0x00400001
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      rdba is 0x00400001 ( FILE 1 BLOCK 1 )

&nbsp;
&nbsp;
### dsr makerdba 5,1   生成对应的RDBA
&nbsp;
&nbsp;

      $ dsr makerdba  5,1
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      rdba is 0x01400001 ( FILE 5 BLOCK 1 )

&nbsp;
&nbsp;
### dsr hxdb o1_mf_system_ld9rh3yb_.dbf 100 以16进制打印一个数据块
&nbsp;
&nbsp;

      $ dsr hxdb o1_mf_system_ld9rh3yb_.dbf 100
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      Offset(h)            |  00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F | 0123456789ABCDEF
      ------------------------------------------------------------------------------------------
      00000000 (00000000)  |  1E A2 00 00 64 00 40 00 CB 00 00 00 00 00 01 04 | ....d.@.........
      00000010 (00000016)  |  54 82 00 00 01 00 00 00 80 C0 EF 02 00 00 00 00 | T...............
      00000020 (00000032)  |  00 00 00 00 00 F8 00 00 00 00 00 00 00 00 00 00 | ................
      * (lines skipped)
      00001FF0 (00008176)  |  00 00 00 00 00 00 00 00 00 00 00 00 01 1E CB 00 | ................




&nbsp;
&nbsp;
### dsr parsenum 0xc202 解析oracle的数字存放格式到10进制
&nbsp;
&nbsp;

      SQL> select dump(100,16) from dual;
      
      DUMP(100,16)
      -----------------
      Typ=2 Len=2: c2,2
      
      
      [oracle@ora7 datafile]$ dsr parsenum c202
      DBSHUIRI DSR 2309 Copyright (c) 2013, 2023, DBRECOVER.COM.  All rights reserved.
      Free for personal use; commercial users need a license.
      
      the number is 100
