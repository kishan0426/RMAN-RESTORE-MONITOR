#!/bin/bash
============================================================
      Date:02-08-2021 Author:Kishan M
============================================================
_env(){
RESTORE_PROGRESS=/tmp/restore_progress.log
export PATH=/apps01/product/12.1.0/dbhome_1/bin:/usr/sbin:/usr/lib64/qt-3.3/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/oracle/.local/bin:/home/                         oracle/bin
export ORACLE_HOME=/apps01/product/12.1.0/dbhome_1
export ORACLE_SID=orcl19x
touch dfsize
export SIZELOG=dfsize
}
_restore_pct(){
while true;do
date_is=$(date "+%F-%H-%M-%S")
#ela_s=$(date +%s)
echo "============================================================"
echo "     ----->$ORACLE_SID<-----"|tr 'a-z' 'A-Z';echo "Restore progress ($date                         _is) "
echo "============================================================"
$ORACLE_HOME/bin/sqlplus -S "/ as sysdba" << EOF
set feedback off
set lines 200
set pages 1000
set termout off
col INPUT_BYTES/1024/1024 format 9999999
col OUTPUT_BYTES/1024/1024 format 9999999
col OBJECT_TYPE format a10
set serveroutput off
variable s_num number;
BEGIN
  select sum((datafile_blocks)*8/1024) into :s_num from v\$BACKUP_DATAFILE;
  dbms_output.put_line(:s_num);
END;
/
set feedback on
select INPUT_BYTES/1024/1024 as inp_byte,OUTPUT_BYTES/1024/1024 as out_byte,OBJECT_TYPE,100*(MBYTES_PROCESSED/:s_num) as pctcomplete from v\$rman_status where s                         tatus like '%RUNNING%';
EOF
#cat $SIZELOG|grep -v 'PL'
#cat /dev/null > $SIZELOG
sleep 6
done
}
#ela_e=$(date +%s)
#echo "elapsed_time: $($ela_e - $ela_s)
_env
_restore_pct
