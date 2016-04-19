#### psawsdbindep
#####start
######compare
RDS->optional redundancy  
DynamoDB->Native redundancy  
Redshift->Native redundancy  

#####creating instances
######create a mysql instance
name:aws
######importing data
for little to no downtime:  
- create database copy
- copy data to EC2
- IMPORT DATA TO NEW RDS INSTANCE
- replicate changes to new instance
- redirect application   
  
#####Lifecycle
######restore a backup
instance actions->restore to point in time.name: awsbackup
######creating and restore
RDS->snapshot->select a snapshot->copy snapshot
######read replica
- async replication
- for mysql or postgresql
- scale high triffic instances  

read replica(up to 5 per instances)
- taken from maz instances
- can add indexes
- cross region replication (mysql only)  

######creating replica
instance actions->create read replica->should be at least same size as original
######multi az deployment
instance actions->modify->multiaz(yes)  
reboot->check reboot with failover  
#####Amz dyn
######native capa
- automated creation
- schema less
- ssd storage
- data replication
- transparent partitioning  

######create table
hash primary key: unique required ,single attribute (best when have known key)  
hash and range primary key(comprised of two attributes, combination must be unique,unordered index + sorted range index
######attr data types
scalar:String Number Binary Boolean Null  
Multi:String Set Number Set Binary Set  
Doc: List Map
######th puts
reads 4kb writes1kb  
100 eventually consist 4kb reads /second (50 read capacoty units)  
100 strongly consist 3kb reads /second (100 read capacoty units,3kb will round to 4kb)  
100 2kb items retrived via query?(100*2/4k = 50 read capacity units)
100 1.1kb write(100*2=200 write capacity unit,round up to 2)  
a read capacity unit equal to one stongly consistent or two eventually consistent 4k reads  
######how partition works
base on hash key.  
single par table 10GB  
number of partition is greater of (read cap units /3000) +(write cap units /1000)  Or table size in bytes /10GB  
unused capacity reserved for bursts
######importing data
create table name:users primary key userid.  
insert data:
```
{
"userid":"1",
"username":"a",
"phone":"123",
"email":"a@a.com",
"fav":"a",
"team":"t"
}
```
######query and scan
getitem: usnig pk,eventually consistent by default.  
query: find item via primary key attributes for table or index (retrived in sorted order)  
scan: reads every item in a table or index(slow as table grows)
######purpose second index
max 5 second indexes per table
######global secondary indexes
for querying nonkey attr  
hash key or hashrange key
projected attr copied into index  
query or scan(no get)
######create second index
table:scores_05 primary:gameid  
global second:gamedate sortkey:sport (project attr:all)
######local sec index:
use alternate range key for hash key  
applied when creating table
######diff
GSI: hash or hashrange, nosize limit ,add later, query all partitions, eventually consistent queryies, dedicated th units, only access projected items.  
LSI: hashrange, foreach 10GBmax, add during table create, query single partition, even/str consist queries,use table through unit, access all attr in table.
#####amz redshift
######load data from integrate data
s3:copy
dynamodb:copy,matches attr name  

######best practice
validate using NOLOAD.  
VACUUM and ANALYZE after load  
merging using staging tables
######demo
syntax,again:
```
copy tbname 'dynamo://tbname' credentials 'aws_access_key_id=123:aws_secret_access_key=456' readratio 50
```
create table in redshift first before importing data   
s3:
```
copy tbname 's3://../csv' credentials '...' ignoreblanklines csv timeformat 'auto'
```
error
```
select * from stl_load_errors;
```
######demo create snapshot
select cluster->backup->config cross r snap




