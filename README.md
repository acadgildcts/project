# project
A = load 'hdfs:/flume_import/StatewiseDistrictwisePhysicalProgress.xml' using org.apache.pig.piggybank.storage.XMLLoader('row') as (x:chararray);

B = foreach A generate REPLACE(x,'[\\n]','') as Y;

C = foreach B generate REGEX_EXTRACT_ALL(Y,'.*(?:<State_Name>)([^<]*).*(?:<District_Name>)([^<]*).*(?:<Project_Objectives_IHHL_BPL>)([^<]*).*(?:<Project_Objectives_IHHL_APL>)([^<]*).*(?:<Project_Objectives_IHHL_TOTAL>)([^<]*).*(?:<Project_Objectives_SCW>)([^<]*).*(?:<Project_Objectives_School_Toilets>)([^<]*).*(?:<Project_Objectives_Anganwadi_Toilets>)([^<]*).*(?:<Project_Objectives_RSM>)([^<]*).*(?:<Project_Objectives_PC>)([^<]*).*(?:<Project_Performance-IHHL_BPL>)([^<]*).*(?:<Project_Performance-IHHL_APL>)([^<]*).*(?:<Project_Performance-IHHL_TOTAL>)([^<]*).*(?:<Project_Performance-SCW>)([^<]*).*(?:<Project_Performance-School_Toilets>)([^<]*).*(?:<Project_Performance-Anganwadi_Toilets>)([^<]*).*(?:<Project_Performance-RSM>)([^<]*).*(?:<Project_Performance-PC>)([^<]*).*');

D =FOREACH C GENERATE FLATTEN (($0));

store D into 'hdfs:/parsefile' using PigStorage(',');
