# BAT_Scanner
#!/bin/bash

#Created by: Mr.Khan
#Created on: 29-July-2022

#Banner of the script
echo "++++++++++++++++++++++++++++++++++++++++++++++++"
echo "==============BAT_Scanner======================="
echo "++++++++++++++++++++++++++++++++++++++++++++++++"

Privillage="sudo"
OUT="BAT_Scanner.log"
BAT="BAT_Output.txt"


echo "=============Script_Started==========="
echo "%%%checking for old logs"
rm="rm -r BAT_*"

if [ `ls -1 BAT_* 2>/dev/null | wc -l` -gt 0 ];
then
    echo "old logs found and removed"
    $rm
else
    echo "old logs not found, Skipping to nmap"
fi

echo

echo "=======Live_host_Discovery==========="

while true; do echo -n %; sleep 1; done &
trap 'kill $!' SIGTERM SIGKILL

$Privillage nmap -sn $1 >> $OUT

echo

echo "=========TCP_NULL_Scan===============" 

$Privillage nmap -sN $1 >> $OUT

echo

echo "=========TCP_Xmas_Scan===============" 

$Privillage nmap -sX $1 >> $OUT

echo

echo "=========TCP_ACK_Scan===============" 

$Privillage nmap -sA $1 >> $OUT

echo

echo "=========TCP_Window_Scan===============" 

$Privillage nmap -sW $1 >> $OUT

echo

echo "======TCP_Version_defaultScripts_OS_Detectionn=====" 

$Privillage nmap -sV -sC -O $1 >> $OUT


echo [100%]Completed

kill $!

#Mofifying the BAT_Scanner.log to only juicy elements

grep -i 'host up' BAT_Scanner.log >> $BAT
grep -i 'open' BAT_Scanner.log >> $BAT
cat $BAT

echo "Script Completed"

