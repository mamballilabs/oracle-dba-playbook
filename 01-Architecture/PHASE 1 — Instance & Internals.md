## 1.1 What is an Oracle Database?

👉 **Database = Files on disk**
- Datafiles
- Control files
- Redo log files

👉 **Instance = Memory + Processes**
- SGA + PGA
- Background processes

## The Core Truth
Instance mounts and operates the database.
Without instance → database is just dead files  
Without database → instance is useless

1.2 Components Breakdown
### INSTANCE

**Memory:**
- SGA (shared)
- PGA (per process)

**Processes:**
- Background processes
- Server processes

### DATABASE (Physical)

- Datafiles → actual data
- Control files → metadata about DB
- Redo logs → change history

```sql
#Instance status
SQL> run
  1* select Instance_name, version, host_name, database_type, status from v$instance

INSTANCE_NAME	 VERSION	   HOST_NAME							    DATABASE_TYPE   STATUS
---------------- ----------------- ---------------------------------------------------------------- --------------- ------------
ORCLCDB 	 23.0.0.0.0	   ol9ai.mamballilab.local					    SINGLE	    OPEN

# database status
SQL> SELECT name FROM v$database;

NAME
---------
ORCLCDB

SQL>

#databae files

SQL> SELECT file_name FROM dba_data_files;

FILE_NAME
---------------------------------------------------------------------------------
/opt/oracle/oradata/ORCLCDB/system01.dbf
/opt/oracle/oradata/ORCLCDB/sysaux01.dbf
/opt/oracle/oradata/ORCLCDB/undotbs01.dbf
/opt/oracle/oradata/ORCLCDB/users01.dbf
/opt/oracle/oradata/ORCLCDB/training01.dbf

SQL> SELECT member FROM v$logfile;

MEMBER
---------------------------------------------------------------------------------
/opt/oracle/oradata/ORCLCDB/redo03.log
/opt/oracle/oradata/ORCLCDB/redo02.log
/opt/oracle/oradata/ORCLCDB/redo01.log

SQL> SELECT name FROM v$controlfile;

NAME
---------------------------------------------------------------------------------
/opt/oracle/oradata/ORCLCDB/control01.ctl
/opt/oracle/oradata/ORCLCDB/control02.ctl

SQL>

SQL> SELECT group#, status, members FROM v$log GROUP BY group#, status, members;

    GROUP# STATUS	       MEMBERS
---------- ---------------- ----------
	 1 CURRENT		     1
	 2 INACTIVE		     1
	 3 INACTIVE		     1

SQL> SELECT group#, member FROM v$logfile ORDER BY group#;

    GROUP#
----------
MEMBER
---------------------------------------------------------------------------------
	 1
/opt/oracle/oradata/ORCLCDB/redo01.log

	 2
/opt/oracle/oradata/ORCLCDB/redo02.log

	 3
/opt/oracle/oradata/ORCLCDB/redo03.log


SQL>

#check SGA

SQL> SHOW SGA;

Total System Global Area 7412633896 bytes
Fixed Size		    5026088 bytes
Variable Size		 1258291200 bytes
Database Buffers	 6140461056 bytes
Redo Buffers		    8855552 bytes
SQL>

#check background processes

SQL> SELECT name FROM v$bgprocess WHERE paddr IS NOT NULL;

NAME
-----
FMON
PMON
DIA5
DIA7
LMD6
LMD8
LMDc
LMDg
LMDl
LMDo
LMDq
LMDr
LMDs
LMDv
LMDx
LCK1
DBWa
DBWg
DBWu
DBWw
BW38
BW49
BW69
BW71
BW78
BW90
BW91
BWad
BWaq
BWba
BWbu
BWbz
BWcc
BWcq
BWdb
BWde
BWdg
BWdn
BWdx
BWee
BWem
BWez
ARC5
ARCj
NSS1
NSS2
NSSA
LSP1
SMON
IMCO
LREG
CJQ0
RSM0
NSVB
NSVS
BG00
LG00
BG02
BG03
BG03
BG01
P008
P00B
J004
J005
J00A
QM02
Q005
IR01
CL00
GEN0
GENS
VKRM
RPOP
DIA0
DIA1
DIA2
DIA3
LMON
LMDd
LMFC
MMAN
DBW3
DBWn
DBWr
DBWt
BW44
BW55
BW62
BW67
BW70
BW81
BW85
BW88
BWae
BWap
BWar
BWbl
BWbn
BWbw
BWcb
BWcg
BWck
BWcn
BWdd
BWdo
BWds
BWeh
BWel
BWeu
BWew
BWex
ARC4
ARCb
ARCc
ARCh
ARCp
NSS5
TPZ1
LSP0
LSP2
GTX4
GTX8
GTXf
GTXh
PXMN
NSVH
NSVM
NSVU
GMON
VBG0
XDMG
EDSB
GCR0
BG01
BG03
BG00
BG00
P001
P004
TT01
M001
M007
J003
J006
GCR1
GWPD
PING
LMD3
LMDe
LMDi
LMDy
DBWe
DBWf
DBWq
BW43
BW53
BW56
BW64
BW75
BW77
BW79
BW95
BW98
BWah
BWak
BWau
BWbj
BWce
BWcl
BWcm
BWcy
BWdk
BWdy
BWep
BWes
ARC6
ARC9
ARCg
ARCl
ARCs
NSS3
CKPT
NSV6
NSV7
NSVE
NSVG
NSVK
NSVP
ARB9
VBG3
VBG5
VDBG
EGSB
SCMN
M000
BG03
S000
P000
P005
P00A
M002
M003
M004
M006
J00C
W001
OFSD
IPC0
SVCB
DSKM
BRDG
DIA6
LMD1
LMD4
LMDj
DBWd
DBWh
DBWv
DBWx
BW36
BW41
BW42
BW46
BW47
BW72
BW73
BW93
BW94
BWan
BWao
BWay
BWbo
BWbp
BWbt
BWcs
BWcu
BWcz
BWdf
BWdj
BWdr
BWec
BWeg
BWet
ARC0
ARC8
ARCa
ARCe
ARCm
ARCn
NSS8
TPZ2
ABMR
RVWR
GTX1
GTXe
GTXg
NSV2
NSV3
NSV8
NSVO
ARB0
ARB3
ARB7
MARK
SCRB
SCMN
BG01
W002
SCMN
BG02
SCMN
BG03
DT00
TT00
M005
J009
Q003
Q001
IR02
VOSD
RSMN
DIA4
LMD2
DBW0
DBW4
DBWi
DBWm
DBWy
DBWz
BW37
BW40
BW45
BW48
BW58
BW60
BW61
BW63
BW65
BW68
BW80
BW86
BW99
BWai
BWaj
BWal
BWam
BWas
BWbd
BWbi
BWbm
BWbr
BWbs
BWbv
BWby
BWca
BWco
BWdh
BWei
ARC2
ARC7
NSS6
NSS7
NSS9
LCK0
FBDA
RECO
GTXd
NSV4
NSV5
NSVD
NSVI
ARB6
FENC
VBG2
VBG4
VBG8
VMB0
XDWK
SCMN
BG00
SCMN
BG01
BG02
J000
J001
Q002
DIAG
CLMN
NMON
LMD7
LMDb
LMDf
LMDh
RMS0
LMHB
DBW2
DBW5
DBWs
BW50
BW76
BW89
BW96
BWac
BWag
BWbg
BWbq
BWci
BWcj
BWcr
BWcv
BWcx
BWdc
BWdw
BWef
BWej
BWek
BWeo
BWeq
BWey
MRP0
TMON
ARCd
ARCf
ARCq
LGWR
CTWR
GTX2
GTX7
GTXa
AQPC
NSVA
NSVF
NSVL
NSVN
NSVQ
FSFP
ASMB
AMB1
VBG6
ACFS
MMNL
BG00
BG00
BG02
BG03
BG02
P003
P006
J002
J008
Q004
GEN2
PSP0
RMON
PMAN
LMDa
LMDm
LMDn
LMDp
LMDt
LMDw
LMDz
DBW7
DBW9
DBWc
DBWl
BW52
BW54
BW66
BW82
BW83
BW84
BW87
BW92
BW97
BWaa
BWab
BWaf
BWat
BWav
BWaw
BWbb
BWbe
BWbf
BWbh
BWcd
BWcf
BWch
BWcp
BWct
BWda
BWdi
BWdl
BWdt
BWdu
BWdv
BWdz
BWen
ARCi
ARCo
ARCr
ARCt
RTTD
SMCO
GTX3
GTX6
GTXi
DMON
NSV0
NSV1
NSVC
NSVJ
INSV
ARB2
ARB5
ARBA
VBG1
VBG7
VUBG
BG01
BG01
LG01
BG02
GCR2
DT01
BG00
D000
P002
P007
J007
VKTM
TES2
UTMU
DBRM
ACMS
DIA8

NAME
-----
DIA9
LMD0
LMD5
LMD9
LMDk
LMDu
DBW1
DBW6
DBW8
DBWb
DBWj
DBWk
DBWo
DBWp
BW39
BW51
BW57
BW59
BW74
BWax
BWaz
BWbc
BWbk
BWbx
BWcw
BWdm
BWdp
BWdq
BWea
BWeb
BWed
BWer
BWev
ARC1
ARC3
ARCk
NSS4
TPZ3
GTX0
GTX5
GTX9
GTXb
GTXc
GTXj
RCBG
NSV9
NSVR
NSVT
RBAL
ARB1
ARB4
ARB8
AMB2
AMB3
VBG9
MMON
BG00
BG00
BG01
BG01
GCW0
P009
J00B
IR00

561 rows selected.

SQL>

```

LAB 2 — CONTROL THE INSTANCE
```sql
#shutdown 
SQL> shutdown immediate;
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL>

#start in nomount:
SQL> STARTUP NOMOUNT;
ORACLE instance started.

Total System Global Area 7412633896 bytes
Fixed Size		    5026088 bytes
Variable Size		 1258291200 bytes
Database Buffers	 6140461056 bytes
Redo Buffers		    8855552 bytes
SQL>

SQL> SELECT status FROM v$instance;

STATUS
------------
STARTED

SQL>
```

what is actually running right now?
- Memory is allocated - SGA is created
- Background processes are started - PMON,SMON,DBWn,LGWR...

what is not accesses yet?
- control file - not read
- datafiles - not opened
- redo logs - not touched

core truth:
At nomount, oracle has no idea what database it will open 
It only knows:
- parameter file
- memory configurations
- process structure
```sql
SQL> SELECT name FROM v$database;
SELECT name FROM v$database
                 *
ERROR at line 1:
ORA-01507: database not mounted
Help: https://docs.oracle.com/error-help/db/ora-01507/


SQL>
```

what oracle used for start here:
At nomount, oracle reads the SPFILE/PFILE only

Why nomount exists:
1. creating databsae
2. recreating control file
3. certain recovery operations

|Stage|What Exists|What Oracle Knows|
|---|---|---|
|NOMOUNT|Instance only|Parameters only|
|MOUNT|+ Control file|Structure of database|
|OPEN|+ Datafiles|Full database access|

MOUNT:
```sql
SQL> ALTER DATABASE MOUNT;

Database altered.

SQL>

SQL> SELECT status FROM v$instance;
SELECT name FROM v$database;
STATUS
------------
MOUNTED

SQL>

NAME
---------
ORCLCDB

SQL>
```


