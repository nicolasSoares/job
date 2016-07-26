Part I:

Context :

	The company :
	Société Générale is one of the main and oldest french multinational banking and financial services company.
	The company group is a universal bank divided into 3 main pillars :
		- Retail banking (Société Générale, Crédit du Nord and Boursorama)
		- International Banking and Financial Services (IBFS)
		- Corporate and Investment Banking (SG CIB)
	Those 3 pillars are backed by 2 other core activities :
		- Specialised Financial Services & Insurance
		- Private Banking, Global Investment Management & Services
		
	
	The service:
		You are now part of the RESG/GTS/MKT/SSB/GRD department of Société Générale, each acronym represents a layer in the hierarchy of the group :
			- RESG : 
				The direction of the group's ressources and innovations, it gathers the teams in charge of information systems, technical infrastructures, purchases, estate operations and counsel activities of the group
				
			- GTS (Global Technology Service) :
				GTS ensures the proper functioning of computer infrastructures and their resilience and counsel and assist the business lines and functionnal divisions for the implementation of their projects
				The computer infrastructures handle the workstations, the computing centres hosting the servers and the computer and telecom networks.

			- MKT (Market) :
				The wholesale bank division, its goal is to ensure the proper functioning of the computer infrastructures and the client relationship with its business partners SG CIB (Corporate and Investment Banking business of Société Générale) and GIMS (cover the private bank activities, asset management and securities services)
				This division assists its clients for their infrastructures choices, coordinates changes and ensures projects monitoring, service quality and respect of financial commitments.
				
			- SSB (Servers, Storage and Backup) :
				This division is in charge of operations on storage et backup of the servers. The team assure the administration, maintenance and backup of all the Market infrastructure (physical and virtual) and handles several projects to ensure the proper functioning of the production
				
			- GRD (Grid) :
				The Grid team is in charge of handling and monitoring the grid computing platform and infrastructure for various departments of Société Générale

(
		Each of those departments except GRD contains several other departments, for example:
			RESG/GTS/TFO (Technical Foundations) : They handle the networks and telecoms of the group and manage the activity continuation plans and the infrastructure's resilience.
			RESG/GTS/EUS (end User Services) : They ensure the proper functioning of the end users tools (workstations, phones, printer etc...)
)

			Grid computing :
		Grid computing is used for various purpose demanding high compute power including financial risk and pricing computing.
		To achieve this high compute power, the grid computing platform is composed of a set of heterogenous and affordable computing resources, working together to provide clients a unified compute power source.
		The computing resources are glued together and handled by middleware programs.
		The main middleware software used in the grid is the Symphony platform (IBM), designed for the financial services industry and working with various operating systems.
		
	
	
The projects:

		You will be working on the Grid monitoring projects, your mission is to continue the developement of exisiting tools and develop new projects or features of projects, mainly using the C# language, according to the needs of the Grid team's clients.


		The projects, stored in a subversion repository, are all in the same solution with the several tools they use.
		To work on the projects, you'l have to run the "SetupDrivesAndStartVS2010.bat" script in the "scripts" folder, it will mount the virtuals X and Y disks, used by the projects so they have the same environment as in production, and laucnh the solution in Visual Studio.

		The solution contains several projects and tools, in the first time you will continue the development for the GridAudit and GridSupervisor projects.
			Those projects have several dependencies common the other projects, therefore those dependencies are grouped in projects :
				GTS-Common-Libs and GTS.GRD contains various helpers, class extensions and configurations
				GTS.GRD.Domain contains classes and configurations used to communicate with the Grid database with the ORM NHibernate
				GTS.GRD.Interfaces contains the interfaces used for the IoC (see below) of the services
				GTS.GRD.Services includes several classes managing services (persistence, checks, WMI requests, notifications, compute nodes etc...) 
			Most of the classes of the projects use the Inversion of Control design principle (IoC), it uses dependency injection to handle the load and configuration of the dependencies of the classes at run time.
			This design principle allow a more modular approch and prevents thight coupling between classes.
			The class IoCManager handles it, it uses the Castle Windsor Inversion of Control container one of the main IoC framework for .NET
			
			GridAudit and GridSupervisor
		Common:
	GridAudit:
	
	GridAudit is a console program that will apply several tests and requests regarding a given list of server nodes to acquire and store informations about those nodes and detect inconsistencies among the differents sources of those informations. ~~~~
	
	Usage:
		The list of server nodes is given to the program by parameter, either by giving directly a coma separated list of node name with "-Node" or by giving the path to a file containing the list of node with "-File".
		The "-FromGridDB" or "-FromMarleyDB" parameters alone can also be used to select all nodes listed respectively in the Grid database or in the Marley database.
		The parameter "-NoPostCheck" can be added to disable the execution of the post-checks.
	
	functioning, titre a trouver:
		After parsing the parameters the program call the Process method of the GridAudit class with a list of node name.
		For each node a thread is created, each thread will execute a list of check regarding a server node.
		The check list is defined in the configuration file and they will be executed in the same order as listed in the configuration file.
		A check can contain nested checks that will be only executed if the parent check has been executed succesfully and return a positive value.
		(for example with a WMI check containing all the WMI related checks, it don't need to try to execute all the nested checks for a node not compatible with WMI)

		The currents checks are divided in 2 types:
			parent checks used as conditions for the execution of their nested checks:
				NodeWmiCheck: verify if the node checked can receive WMI requests
				NodePingState: verify if the node can be pinged

			checks retreiving informations about the node and storing/updating those informations in the Grid database to be accesible in the GridHome platform:
				NodeExistsInInfos : test if the node is listed in the Grid database or/and in the Marley database
				NodeHalInfos : retreive several informations about the node from the HAL API (server installation date, decomission date, bundle etc...)
				NodeHardwareInfos: retreive several hardware and operating system informations about the node, mainly through WMI requests (Os version/architecture, model, memory informations, uptime etc...)
				NodeIcmInfos: get the node compliance ratio from the ICM API
				NodeIKnowInfos: test if the node is listed in the IKnow API
				NodeMarleyInfos: retreive several hardware and operating system informations about the node from the Marley database
				NodeSymphonyInfos: retreive Symphony informations from the node
				
		Once all the checks have been executed for each node and each thread is killed, a series of post-checks (defined in the configuration file like the previous checks) are executed (except if the "-NoPostCheck" option was passed to the program).
		Contrary to the previous checks, the post-checks are not executed for each node, they use directly the full list of nodes.
		The post-checks search for inconsistencies of result from the checks :
			CpuRepositoryDatabasePostCheck : list the entries  in the table AuditCpuRepository (Grid database) created automatically that should be checked by a human to confirm the number of CPU/cores available
			LocationPostCheck: check location consistencie in the AuditNode table of the Grid database
			NoMissingGridNodePostCheck : list all the symphony nodes that should have been listed in the auditNode table
			RecentUpdatesPostCheck: lists the outdated entries from the AuditNode table
		When a post-check found inconsistencies an alert is send by mail (the recipient is defined in the configuration file)
		
		
	GridSupervisor:
		GridSupervisor run as a service (except if it is executed with the "-NoDebug" parameter) and execute a list of compliance check on lists of server nodes.
		The compliance errors detected are stored in the "GridComplianceAlerts" table of the Grid database to be displayed on the GridHome platform
		The nodes lists are loaded from a configuration file, each list has its own list of check.
		The table "ref_GridCompliance" of the Grid database contains the checks configurations : 
			scope, delay between notifications, actions to execute if an alert is raised, templates to use on the GridHome platform.

	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
-------------------------------------------
Grid Computing technical scope :

Some applications want to use as much computing resources as possible : 
 CPU power is the bottleneck

Computing resources can be connected and work together 
 Glued by a middleware

 
 
 
 
Areas where Grid Computing is used 

Science & numerical simulations 
 Molecular biology, weather forecast, seismology

Industry 
Complex modeling, finite elements computation

Finance 
Risk evaluation, pricing

Gaming industry


 
 grid computing: A set of heterogenous & affordable computing resources, working together to provide clients a unified compute power source

 
 A software solution for applications that require intensive computation.
Provides a robust, affordable and highly scalable environment
Leads to faster and more reliable results for clients


Grid Computing = Distributed Computing + resource and workload management (control is very important note de gty)


Management of 
Resources 
Virtualization, dynamic allocation and ownership
Monitoring & control
Resource Guaranteed SLA (Service Level Agreement) management
Workload
Execution, monitoring & control
Scheduling and prioritization with load balance
High reliability, availability, and resilience
… while maintaining performance and scalability


A single grid solution recommended within ITEC since 2003: Platform Symphony
A global standard as the primary recommendation for projects
Platform Symphony selected by “almost” all Grid projects 
CRS/RIS using Platform LSF (batch oriented solution)
Confirmed as unique solution during IDEA BC2R3 benchmark last July 2006
Significant Grid projects engagement
Large departmental grids in production 
Significant investment in this area
Transversal grid support: GTS Grid Services



grosse part server sont symphony (note de gtry)


Space limitation in datacenters => Outsourcing
Data access from grid applications => DataGrid (GigaSpaces)
Becoming a standard for big grids (Bacardi, AgRisk)
Better utilization of current resources => Grids mutualization
Merge of ex-GEDS post-trade grids => 10 000 cores on 1 grid
Study starting (GTS/MKT and ITEC)
New technologies => GPU
Technology watch in progress, especially within MARK/R&D
Won’t be used in short/middle term (complexity in development, not fully mature)



scope:  application : ITEC
		<===>
		grid middleware: gtts grid
		<====>
		os hardware windows unix teams
		
		
grid team:: Expertise and Support on Platform Symphony and LSF grid computing middleware
5 FTE
Responsible: Anthony Fréry

Experts: Eric Fauquembergue / Benoît Serratrice / Alexandra Minne / Nicolas Jacq
Services
Advise and support applications & BLs teams at all stages (conception, development, production)
Cluster management: Installation / upgrade, incident management, vendor relation
On duty support (24x7)
Many tools: monitoring, reporting, administration portal, documentation, etc


services: ITEC/BLs advise
During conception/development: About architecture, infrastructure, etc.
Training sessions
Clusters management
Install/Upgrade clusters, deploy hotfixes
Administrate cluster in coordination with ITEC/BLs
Incidents diagnostic
Analyze/Solve clusters’ issues
Vendor relation
Manage support tickets
Follow-up products evolution
On duty
Grid team can be called on duty 24x7
Monitoring of production clusters is configured to alert Grid team on duty


tools: GridHome - http://gridhome.fr.world.socgen/
Intranet web site to manage grids
Audience : ITEC/BLs support
2 usages :
View: display information about clusters. (example: topology, healing center)
Manage: execute maintenance actions on clusters. (example: monitoring/unmonitoring, open/close/restart nodes)
Monitoring
Full monitoring  of grid middleware
Raise alerts to grid team and/or ITEC/BLs support

Reporting - http://gridhome.fr.world.socgen/Reports/ and http://gridreporting.fr.world.socgen
Information about clusters: topology, dashboard, workload, CPU usage, etc.
Data is also provided through web services
Grid agility pack
A set of tools to accelerate and facilitate grid adoption
Wiki - http://wiki.fr.world.socgen/display/GRID
Description of grid team tools
Procedures of maintenance actions




notes pour moi::

sur le projet: parler du nfutur du projet ce que devra faire le nouveau

idée faire le rapport sous forme de mail pour le nouveau








 
 


