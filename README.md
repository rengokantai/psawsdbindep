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

