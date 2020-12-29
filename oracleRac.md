## Oracle Clusterware
+ In the current release, Oracle has formally divided the Oracle Clusterware Technology stack into separate stacks.
+ The ```Cluster Ready Services (CRS)``` is upper technology stack and the ```Oracle High Availability Services (OHAS)``` stack constitutes the foundational or lower layer.

## The Cluster Ready Services (CRS)
+ CRS : This is the key service responsible for managing Oracle RAC high availability operations. The CRS daemon, named CRSd, manages the cluster’s start, stop, and failover operations. CRS stores its configuration data in the Oracle Cluster Registry (OCR).
+ CSS : The CSS service manages the cluster’s node membership and runs as the ocssd.bin process in a Linux-based system.
+ EVM : The Event Manager service (EVMd) manages FAN (Fast Application Notification) server callouts. FAN provides information to applications and clients about cluster state changes and Load Balancing Advisory events for instances services and nodes.
+ Oracle Notification : The ONSd daemon represents the Oracle Notification Service, which, among other things, helps publish the Load Balancing Advisory events.
+ Oracle installs three wrapper scripts: init.crsd, init.evmd, and init.cssd.
+ Failure of OCSSd causes the machine reboot to avoid a split-brain situation.

## Cluster Membership Decisions
+ contains logic for determining the health of other nodes of the cluster and whether they are alive.

## Resource Management Frameworks
+ Correct modeling of resources stipulates that they must be managed by only one cluster frameworks.

## Starting and Stopping Oracle Clusterware
+ when a node fails, Oracle Clusterware is brought up at boot time via the init daemon. Therefore, if the init process fails to run on the node, the OS is broken and Oracle Clusterware does not start.
```
crsctl stop crs
crsctl start crs
crsctl enable crs
crsctl disable crs
```
+ Only one set of CRSs can be run on one cluster. Adding the ```-cluster``` option to these commands make then global across the RAC environment.
+ You must use ```SRVCTL``` to start and stop Oracle resources named with ```ora.``` Oracle does not support third-party applications that check Oracle resources and take corrective actions on those resources. Best practice is to leave Oracle resources controlled by Oracle Clusterware.

## The Clusterware Startup Process
+ The ```The Oracle High Availability Service daemon (OHASd)``` starts all other Oracle Clusterware daemons.
+ The ```/etc/inittab``` file executes the ```/etc/init.d/init.ohasd``` control script with the ```run``` argument, which spawns the ohasd.bin executable.
+ The cluster control files are stored at the location
```/etc/oracle/scls_scr/<hostname>/root```. The ```/etc/init.d/init.ohasd``` control script starts the OHASd based on the value of the ohasdrun cluster control file.
+ The value ```restart``` causes Oracle to restart the crashed OHASd using ```$GRID_HOME/bin/ohasd restart```.
+ The value ```stop``` indicates a scheduled shutdown of the OHASd.
+ The value ```reboot``` causes Oracle to update the ohasdrun file with a value of ```restart``` so that Oracle will restart the crashed ```OHASd```.
+ The OHASd daemon uses cluster resources to start other Clusterware daemons. The OHASd daemon will have one cluster resources for each Clusterware daemon, and these resources are stored in the Oracle Local Registry.
+ agents help manage the Clusterware deamons, by performing actions such as starting, stopping, and monitoring the Clusterware daemons.
+ The four main agents are ```oraagent```, ```orarootagent```, ```cssdagent```, and ```cssdmonitor```. These agents perform start, stop, check, clean actions on their respective Clurster daemons.

## Oracle Cluster Registry
+ store meatadata, configuration, and state information of all the cluster resources defined in Oracle Clusterware. ```Oracle Cluster Registry(OCR)``` must be accessible to all nodes in the cluster.
+ The ```OCR``` is used to bootstrap ```CSS``` for port information, nodes in the cluster and similar information.
+ The ```Cluster Synchronization Services daemon(CSSd)``` updates the ```OCR``` during cluster setup and once the cluster is set up, OCR is used for read-only operations. Exceptions are any change in a resource status that triggers an OCR update, services going up or down, network failovers, ONS and a change in application state, and policy changes.
+ ```OCR``` is the central repository for ```CRS``` and it stores details about  the services and status of the resources.
+ Oracle stores the location of the OCR file in a text file called ```ocr.loc```, which is located in different places depending on the operating system.
+ On Linux-based systems the ```ocr.loc``` file is placed under the ```/etc/oracle``` diretory.
+ Oracle uses an in-memory copy of ```OCR``` on each cluster node to optimize the queries against the ```OCR``` by various client, such as ```CRS, CSS, SRVCTL, NETCA, Enterprise Manager and DBCA```. Each cluster node has its own private copy of the ```OCR```, but to ensure the atomic updates against the ```OCR```, no more than on ```CRSd``` process in a cluster is allowed to write into the shared ```OCR``` file.
+ This master ```CRSd``` process refreshed the ```OCR``` cache on all cluster nodes.
+ Clients communicate with the local ```CRSd``` process to access the local copy of the ```OCR``` and to contact the master ```CRSd``` process for any updates on the physical OCR binary file.
+ The ```OCR``` file is automatically backed up in the ```OCR``` location every four hours. These backups are stored for a week and circularly overwritten. The last three successful backups of ```OCR``` are always available in the directory ```$ORA_CRS_HOME/cdata/<cluster name>```. The ```OCR``` backup location can also be changed using the ocrconfig command-line utility.
+ You can also take a manual backup of the ```OCR``` binary file by executing the ```ocrconfig -manualbackup``` command, provided that Oracle Clusterware is up and running on the cluster node. The user running the ```ocrconfig``` command must have administrative privileges. The ```ocrconfig``` command can also be used to list the exsting backups of the ```OCR``` and the backup location, as well as to change the backup location.

## Oracle Local Registry (OLR)
+ The ```OLR``` is similar to the Oracle Cluster Registry, but it only stores information about the local node. The OLR is not shared by other nodes in the cluster and is used by the OHASd while starting or joining the cluster.
+ The ```OLR``` stores information that is typically by the ```OHASd```, such as the version of Oracle Clusterware, the configuration, and so on.
+ Oracle stores the location of the OLR in a text file named ```/etc/oracle/olr.loc```. The file will have the location of the OLR configuration file ```$GRID_HOME/cdata/<hostname.olr>```.

## Voting Disk
+ A votting disk is a shared disk that will be accessed by all the member node in the cluster during the operation.
+ One problem with voting is plurality, the leading candidate gains more votes than the other candidates, but more than half of the total votes cast.
+ Other problems with voting involve ties. Ties are rare, and the node contained in the master sub cluster will survive.
+ A ```vote``` is usually a formal expression of opinion or will in response to a proposed decision.
+ A ```quorum``` is defined as the number, usually a majority of officers or members of a body.

## Cluster Synchronization Services (CSS)
+ It provides the access to the node membership and enables basic cluster services, including cluster group services and cluster locking.
+ Failure of ```OCSSd``` causes the machine to reboot to avoid a ```split-brain``` situation.
+ ```OCSSd``` runs as the ```oracle``` user.
+ The ```OCSSd``` process is spawned by the ```init.cssd``` wrapper script, runs as a non-root operating system user with real-time priority, and manages the configuration of Oracle Clusterware by providing node cluster and group membership services.
+ ```OCSSd``` provides these services via two types of heartbeat mechanisms: <br> 1. network heartbeat <br> 2. disk heartbeat
+ The main objective of the network heartbeat is to check the viability of the Oracle cluster, whereas the disk heartbeat helps to identify the split-brain situation.
+ The ```init.cssd``` wrapper script is configured with the ```fatal``` parameter. Failure of ```OCSSd``` causes the machine reboot to avoid a split-brain situation.
+ The following list summarizes the functionalities of ```OCSSd```: <br>1. CSS provide basic Group Services support. <br>2. Lock Services provides the basic cluster-wide serialization locking functions.<br>3. Node Services uses ```OCR``` to store data and updates the information during reconfiguration.
+ The ```cssdagnet``` process is part of Oracle Synchronization Services, and its job is to continuously monitor the cluster to provide the I/O fencing solution.
+ The prime objective of the ```cssdagent``` process is to identify potential cluster node hangs and to reboot any hanged node so that processes on that node cannot write to the storage.
+ The ```cssdagent``` process is locked in memory and runs as a real-time process. It sleeps for a fixed time and runs as the root user. Failure of the ```cssdagent``` process causes the node to automatically restart.

## The Event Menager Process (EVMd)
+ The third key component in ```OCS``` is called the ```Event Management Logger```.
+ This ```EVMd``` daemon process spawns a permanent child process called ```evmlogger``` and generates the events when things happen.
+ The ```evmlogger``` spawns new children processes on demand and scans the callout directory to invoke callouts.
+ The ```EVMd``` process receives the ```FAN``` events posted by clients and distributes the ```FAN``` events to the clients that have subscribed to them.
+ The ```EVMd``` process will restart automatically on failure, and the death of the ```EVMd``` process does not halt the instances. The ```EVMd``` process runs as the ```oracle``` user.

## I/O Fencing
+ protects processes from other nodes modifying the resource during node failure. When a node fails, it need to be isolated from the other active nodes.
+ Fencing, in general, ensures that I/O can no longer occur from the failed node.

## Oracle Notification services
+ The ```Oracle Notification Services (ONS)``` process is configured during the installation of Oracle Clusterware and is started on each cluster node when ```CRS``` starts.
+ whenever the state of a cluster resource changes, the ```ONS``` process on each cluster node communicates with one another and exchanges the ```HA``` event information.
+ ```CRS``` triggers these ```HA``` events and routes them to the ```ONS``` process, and then the ```ONS``` process publishes the ```HA``` event information to the middle tier.
+ The whole process of triggering and publishing the ```HA``` events is called ```Fast Application Notification (FAN)```.
+ These ```FAN``` events alone are of no use until applications do not have the logic to respond to the ```FAN``` events published by the ```ONS``` process.

#### Cluster Time Synchronization Services (CTSS)
+ Performs time management tasks for the cluster.

#### Grid Naming Service (GNS)
+ In charge of name resolution and it  handles requests sent from DNS servers to the cluster nodes.

#### Oracle Agent (oraagent)
+ Replace the ```RACG``` process in Oracle 11.1 and supports Oracle-specific  requirements.

#### Oracle Root Agent (orarootagent)
+ Lets the ```CRSd``` manage root-owned resources, such as the network.

## The Oracle High Availability Services Technology Stack
+ Is responsible for supporting high availability in an Oracle RAC environment.

#### Cluster Logging Services (ologgerd)
+ This process stores information it receives from the various nodes in a cluster in an ```Oracle Grid Infrastructure Management Repository```, which is a default new Oracle 12c database named ```MGMTDB``` that's automatically created for you when you create an Oracle 12c RAC database.
+ The database is creatd as a ```CDB```, with a single ```PDB```, runs on only one of the nodes in an Oracle RAC cluster.
+ Note that the ```ologgerd``` service will run on only one of the nodes in a cluster.

##### Grid Plug and Play (GPNPd)
+ This process manages the Grid Plug and Play profile and ensures that all nodes in a cluster have a consistent profile.

##### Grid Interprocess Communication (GIPCd)
+ This is a support daemon that underlies Redundant Interconnect Usage, which facilitates IP failover natively, without the need for tools such as bonding. This is also the underlying protocol used by the clusterware to communicate.

##### Oracle Agent (oraagent)
+ Not to be mistaken with the oraagent process on the ```CRS``` technology stack, this process also supports Oracle-specific requirements.

##### Oracle Root Agent (orarootagent)
+ This process is the counterpart to the similarly named agent process in the ```CRS``` technology stack and is meant to help manage root-owned services that run in the ```Oracle High Availability Services Technology stack layer```. This process is required for running resources that need ```root-elevated``` privileges.


## Key Networking Concepts
+ The most important Oracle RAC networking concepts you need to understand: <br> 1. Oracle virtual IPs <br> 2. Application virtual IP <br> 3. SCAN

## Oracle Virtual IP
+ A ```VIP name and address``` must be registered in the ```DNS``` in addition to the standard static IP information.
+ Listener would be configured to listen on ```VIPs``` instead of the pulic IP.
+ Whan a node is down, the ```VIP``` is automatically failed over to one of the other nodes. During the failover, the node that gets the ```VIP``` will ```re-ARP``` to the world, indicating the new ```MAC address``` of the ```VIP```.

## Application IP
+ Oracle also extends this capability to user applications.
+ An application VIP is a cluster resource for managing the network IP address.
+ when an application is built dependent on an ```application VIP(AVIP)```, whenever a node goes down, the application VIP fails over to the surviving node and restarts the application process on the surviving node.
+ Oracle supplies the standard action program script ```usrvip```, which is located under the ```<GRID_HOME>/bin``` directory.
+ Difference between a normal VIP and an application VIP, upon failover to a surviving node, a normal VIP does not accept connections and forces clients to reconnect using another address, whereas an application VIP remains fully functional after it is relocated to another cluster node and it continuously accepts connections.

## Single Client Access Name(SCAN)
+ SCAN is a single network name that resolves to three different IP addresses registered either in DNS or GNS.
+ During installation of Oracle Grid Infrastructure, Oracle creates three ```SCAN listeners and SCAN VIPs```.
+ Oracle fails over the ```SCAN VIP``` and the listener to another node in case of failure.
+ Each database instance registers itself to the local listener and the ```SCAN listener``` using the database initialization parameter ```REMOTE_LISTENER```, because this is the only way the ```SCAN listener``` knows about the location of the database instances.
+ The ```SRVCTL``` utility can be used to manage and monitor the ```SCAN resources``` in the cluster.
+ The following steps explain how a client connects to the database in a cluster using ```SCAN```:<br>1. The ```TNS``` layer retrieves the IP address of the ```SCAN``` from the ```Domain Name Server``` or ```Grid Name Server```. It then load-balance and fails over across the IP address.<br>2. The ```SCAN listener``` is now aware of database in the cluster and will redirect the connetion to the target database node ```VIP```.
