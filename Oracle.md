#### Connecting to the Database with SQL*Plus
+ ORACLE_SID and ORACLE_HOME must be set.
```
setenv ORACLE_SID orcl
setenv ORACLE_HOME /u01/app/oracle/product/12.1.0/db_1
setenv LD_LIBRARY_PATH $ORACLE_HOME/lib:/usr/lib:/usr/dt/lib:/usr/openwin/lib:/usr/ccs/lib
sqlplus /nolog
```

+ Syntax of the SQL*Plus CONNECT Command
```
CONN[ECT] [logon] [AS {SYSOPER | SYSDBA | SYSBACKUP | SYSDG | SYSKM | SYSRAC}]
The syntax of logon is as follows:
{username | /}[@connect_identifier] [edition={edition_name | DATABASE_DEFAULT}]
```

#### About Administrative User Accounts
##### SYS
+ All of the base tables and views for the database data dictionary are stored in the
schema SYS.

##### SYSTEM
+ When you create an Oracle database, the user SYSTEM is also automatically created
and granted the DBA role.

##### SYSBACKUP
+ Oracle Recovery Manager (RMAN) backup and recovery
operations either from RMAN or SQL*Plus.

##### SYSDG
+ Data Guard operations. The user can perform operations either
with Data Guard Broker or with the DGMGRL command-line interface.

##### SYSKM
+ Transparent Data Encryption keystore operations.

##### SYSRAC
+ Oracle Real Application Clusters (Oracle RAC) operations by
connecting to the database by the Clusterware agent on behalf of Oracle RAC
utilities such as SRVCTL

### Operations Authorized by Administrative Privileges
+ Having connected as SYSDBA, user mydba now references the SYS schema, but the
table was created in the mydba schema.
+ Having connected as SYSBACKUP, Assume that the sample user mydba has been granted the SYSBACKUP administrative
privilege.
```
SQL> select sys_context('USERENV','CURRENT_SCHEMA') from dual;
SYS_CONTEXT('USERENV','CURRENT_SCHEMA')
SYS
SQL> select SYS_CONTEXT('USERENV','SESSION_USER') from dual;
SYS_CONTEXT('USERENV','SESSION_USER')
SYSBACKUP
```

#### Using Password File Authentication
+ Preparing to Use Password File Authentication
+ To prepare for password file authentication, you must create the password file, set
the ```REMOTE_LOGIN_PASSWORDFILE``` initialization parameter, and grant privileges.
+ For a policy-managed Oracle RAC database or an Oracle RAC One Node database
with ```ORACLE_SID``` of the form ```db_unique_name_n```, where n is a number, the password
file is searched for first using ```ORACLE_HOME/dbs/orapwsid_prefix```.
+ Administrative privileges cannot be granted to roles, because roles are available only
after database startup. Do not confuse the database administrative privileges with
operating system roles.
+ Viewing Database Password File Members : query the ```V$PWFILE_USERS```.

## Creating and Configuring an Oracle Database
#### About Creating an Oracle Database
+ Select the global database name, which is the name and location
of the database within the network structure. Create the ```global
database name``` by setting both the ```DB_NAME``` and ```DB_DOMAIN```
initialization parameters.
+ Oracle recommends using ```AL32UTF8``` as the database character set.


#### Starting Up and Shutting Down
##### Starting Up a Database Using SRVCTL
+ When Oracle Restart is installed and configured for your database, Oracle
recommends that you use SRVCTL to start the database.
+ Run the ```srvctl start database``` command.

##### About Initialization Parameter Files and Startup
+ In the platform-specific default location, Oracle Database locates your initialization
parameter file by examining file names in the following order.
1. The location specified by the ```-spfile``` option in the SRVCTL commands ```srvctl
add database``` or ```srvctl modify database```.
2. ```spfileORACLE_SID.ora```
3. ```spfile.ora```
4. ```initORACLE_SID.ora```

#### Starting Up an Instance
##### Starting an Instance, and Mounting and Opening a Database
+ This mode allows any valid user to connect to the database
and perform data access operations.
##### Starting an Instance Without Mounting a Database
+
##### Starting an Instance and Mounting a Database
+
##### Restricting Access to an Instance at Startup

##### Starting an Instance and Mounting a Database
+ Opening a database in restricted mode allows database access only to users with both the ```CREATE SESSION``` and ```RESTRICTED SESSION``` system privilege.
+ when the instance is in restricted mode, a database administrator cannot access the instance remotely through an Oracle Net listener, but can only access the instance locally from the system that the instance is running on.
+ use the ```ALTER SYSTEM``` statement to disable the RESTRICTED SESSION feature:
```
ALTER SYSTEM DISABLE RESTRICTED SESSION;
```
##### Forcing an Instance to Start
+ If an instance is running, the force mode shuts it down with mode ```ABORT``` before
restarting it.

#### Altering Database Availability
+ To mount a database to a previously started, but not opened instance.
```
ALTER DATABASE MOUNT;
```
+ To open a mounted database、read only、read write mode.
```
ALTER DATABASE OPEN;
ALTER DATABASE OPEN READ ONLY;
ALTER DATABASE OPEN READ WRITE;
```
+ To place a database into a quiesced state.
```
ALTER SYSTEM QUIESCE RESTRICTED;
```
+ The ALTER SYSTEM QUIESCE RESTRICTED statement may wait a long time for active sessions to become inactive. You can determine the sessions that are blocking the quiesce operation by querying the ```V$BLOCKING_QUIESCE``` view.
```
select bl.sid, user, osuser, type, program
from v$blocking_quiesce bl, v$session se
where bl.sid = se.sid;
```
+ To restore the database to normal operation.
```
ALTER SYSTEM UNQUIESCE;
```
+ To view the quiesce state of an instance.
```
select ACTIVE_STATE from V$INSTANCE.
 NORMAL: Normal unquiesced state.
 QUIESCING: Being quiesced, but some non-DBA sessions are still active.
 QUIESCED: Quiesced; no non-DBA sessions are active or allowed.
```
+ The V$INSTANCE view is queried to confirm database status.
```
SELECT DATABASE_STATUS FROM V$INSTANCE;
```

#### Configuring Database Resident Connection Pooling
##### To enable database resident connection pooling
+ Start SQL*Plus and connect to the database as the ```SYS``` user.
```
SQL> EXECUTE DBMS_CONNECTION_POOL.START_POOL();
```
##### Routing Client Connection Requests to the Connection Pool
+ An easy connect string that enables clients to connect to a database resident connection pool.
```
examplehost.company.com:1521/books.company.com:POOLED
```
+ TNS connect descriptor that enables clients to connect to a database resident connection pool.
```
(DESCRIPTION=(ADDRESS=(PROTOCOL=tcp) (HOST=myhost)
(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=sales)
(SERVER=POOLED)))
```
+ Only the TCP protocol is supported for client connections to a database
resident connection pool.
##### Disabling Database Resident Connection Pooling
+ Start SQL*Plus and connect to the database as the ```SYS``` user.
```
SQL> EXECUTE DBMS_CONNECTION_POOL.STOP_POOL();
```
##### Configuring the Connection Pool for Database Resident Connection Pooling
+ The CONFIGURE_POOL procedure of the DBMS_CONNECTION_POOL package enables you to configure the connection pool with advanced options.
+ changes the minimum number of pooled servers
used
```
SQL> EXECUTE DBMS_CONNECTION_POOL.ALTER_PARAM ('','MINSIZE','10');
```
+ changes the maximum number of connections that each
connection broker can handle to 50000.
```
SQL> EXECUTE DBMS_CONNECTION_POOL.ALTER_PARAM ('','MAXCONN_CBROK','50000');
```
+ ensure that the maximum number of connections allowed by the platform on which your database is installed is not less than the value you set for ```MAXCONN_CBROK```.
##### Restoring the Connection Pool Default Settings
+ use the RESTORE_DEFAULT procedure of the ```DBMS_CONNECTION_POOL``` package.
```
SQL> EXECUTE DBMS_CONNECTION_POOL.RESTORE_DEFAULTS();
```
##### Determining the States of Connections in the Connection Pool
+ Query the ```V$CPOOL_CONN_INFO``` view.

### About Managing Prespawned Processes

### Managing Pools for Prespawned Processes
+ You can use the ```DBMS_PROCESS``` package to configure and modify the number of prespawned processes in the foreground process pool.
+ You can view the current process pools by querying the ```V$PROCESS_POOL``` view.
+ Stopping a Process Pool
```
SYS_DEFAULT_FOREGROUND_POOL
exec DBMS_PROCESS.STOP_POOL('SYS_DEFAULT_FOREGROUND_POOL');
```

### Managing Processes for Parallel SQL Execution
+ The parallel execution feature described in this section is available with the Oracle Database Enterprise Edition.
+ The ```degree of parallelism``` is determined by any of the following:
1. A ```PARALLEL``` clause in a statement.
2. For objects referred to in a query, the ```PARALLEL``` clause that was used when the
object was created or altered.
3. A parallel hint inserted into the statement.
4. A default determined by the database.

### Altering Parallel Execution for a Session
+ You control parallel SQL execution for a session using the ```ALTER SESSION``` statement.
+ Disabling Parallel SQL Execution
```
ALTER SESSION DISABLE PARALLEL DML|DDL|QUERY
```
+ Enabling Parallel SQL Execution
```
ALTER SESSION ENABLE PARALLEL DML|DDL|QUERY
```
+ Forcing Parallel SQL Execution
```
ALTER SESSION FORCE PARALLEL DML DDL|QUERY
```

## Terminating Sessions
+ You terminate a current session using the SQL statement ```ALTER SYSTEM KILL SESSION```.
+ The following statement terminates the session whose system identifier is 7 and serial number is 15.
```
ALTER SYSTEM KILL SESSION '7,15';
```

### Identifying Which Session to Terminate
+ To identify which session to terminate, specify the session index number and serial number.
+ To identify the system identifier (SID) and serial number of a session.
```
//Query the V$SESSION dynamic performance view.
SELECT SID, SERIAL#, STATUS FROM V$SESSION WHERE USERNAME = 'JWARD';
```
+ A session is ```ACTIVE``` when it is making a SQL call to Oracle Database. A session is ```INACTIVE``` if it is not making a SQL call to the database.
+ An active session cannot be interrupted when it is performing network I/O or rolling back a transaction. Such a session cannot be terminated until the operation completes.
+ A session marked to be terminated is indicated in ```V$SESSION``` with a status of ```KILLED``` and a server that is something other than ```PSEUDO```.
+ the following statement specifies that the session will not be recovered.
```
ALTER SYSTEM KILL SESSION '7,15' NOREPLAY;
```
+ to disconnect all sessions with the service ```sales.example.com``` and specify that the sessions should not be recovered.
```
BEGIN
DBMS_SERVICE.DISCONNECT_SESSION(
service_name => 'sales.example.com',
disconnect_option => DBMS_SERVICE.NOREPLAY);
END;
/
```

### Terminating an Inactive Session
+ If the session is not making a SQL call to Oracle Database (is INACTIVE) when it is terminated.
+ the ```ORA-00028``` message is not returned immediately.
+ The message is not returned until the user subsequently attempts to use the terminated session.
+ When an inactive session has been terminated, the ```STATUS``` of the session in the ```V$SESSION``` view is ```KILLED```.
+ The row for the terminated session is removed from ```V$SESSION``` after the user attempts to use the session again and receives the ```ORA-00028``` message.
+ ```V$SESSION``` is queried to identify the ```SID``` and ```SERIAL#``` of the session.
```
SELECT SID,SERIAL#,STATUS,SERVER
FROM V$SESSION
WHERE USERNAME = 'JWARD';
SID SERIAL# STATUS SERVER
7  15 INACTIVE  DEDICATED
12 63 INACTIVE  DEDICATED
ALTER SYSTEM KILL SESSION '7,15';
SELECT SID, SERIAL#, STATUS, SERVER
FROM V$SESSION
WHERE USERNAME = 'JWARD';
SID SERIAL# STATUS SERVER
7  15  KILLED   PSEUDO
12 63  INACTIVE DEDICATED
```
