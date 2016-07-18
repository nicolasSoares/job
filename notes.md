MKT/SSB - Server Storage and Backup team activities
SSB team is in charge of Servers storage and backup activities.
We guarantee the operational maintenance of Server storage and backup infrastructures
We process a lot of projects with and for you (growing, migration, ...) to improve together our information system



The MKT/SRV/SEC team is part of the MKT-SRV-ENG team focused on Operational Security topics within MKT-SRV.
Our mission is composed of the following tasks (non-exhaustive list):
Ensure production stability by following potential threats and taking proper actions (0-day flaws, ...)
Be the point of contact for security concerns inside and outside the team
Evaluate and follow deployment of security patches
Follow security crisis
Drive and follow security projects

Grid Computing is a software solution for compute and data intensive applications, that is robust, highly scalable across a virtualized and affordable infrastructure, delivering faster and more reliable results. 

---

gridAudit::              |
_________________________|

gridaudit take a list a node from a target (from a file , all the nodes from GridDB or Marley or directly as program's arguments)

a thread is executed for each node and a list a audit checks is applied to the node

checks are executed in the same order as in the configuration file
a check can contain nested checks that will be only executed if the parent check has been executed succesfully (return < 0)
(for example a check WMI containing all the WMI related checks, therefore we don't need to try to execute all the nested checks for a node not compatible with WMI)~~


main => {

	if parse
		IOC->IgridAudit // gridaudit // get config
		IgridAudit::Process listServer

}

Parse => {
	// check args
	
	// list servers from args target
	
	// remove postchecks if needed
	
	// print usage if error (wrong arg, wrong file etc..)
}

IgridAudit::Process => {
	//raw for now

	TPW = new ThreadPoolWorker
	nodes.foreach_notnull
		// launch audit thread for each node
		TPW.Start( new ThreadStart( new NodeThread(node, check_configs) ) )
	
	//wait while thread are being executed
	
	if postchecks
		postcheck (from config) .foreach{
			IOC::postcheck
			postcheck.check(nodes)
			if results 
				sendmail
		}
	
	//raw for now

}

//***** POSTCHECKS:: get inconsitencies
CpuRepositoryDatabasePostCheck : log entries in auditcpurepository that have been created automaticly so a human can check that the number of cpu/core of a given architecture can be checked, when a human has checks a cpu it have to mark hasbeenhumanchecked to true so it is not logged again
	cpus = get CPUs with wasHumanChecked set to false
	foreach cpus {
		get audnitnode of cpu type
	}

	
LocationPostCheck:
Implements a class that is capable of checking for LOCATION consistency in AuditNode table of GRID database : location must not be empty, and names must respect the standard


NoMissingGridNodePostCheck:
check if symphony nodes are not in auditnodes (maybe)


RecentUpdatesPostCheck



**********************************************************************************************
**********************************************************************************************
**********************************************************************************************
**********************************************************************************************
**********************************************************************************************
**********************************************************************************************


lancé en tache de fond, service, hors debug mode

config: liste de cluster (avec des regles d'inclusions et d'exclusion)
chaque cluster contient une liste de check
les parametres des checks sont récupéré en bdd


while true {
	process

}


process() {

	foreach cluster //(config)  remove dead cluster and excluded clusters // if dead sendmail
	{
		foreach node in nodesofcluster { // test if each node is reachable
			check status, ping, testwmi
		}
		
		if cluster is not in actionsdone => add to actionsdone
		
		if error before sendmail
		
		foreach cluster.checks {
			if check  is not in actionsdone[cluster] => add to actionsdone[cluster]
			
			foreach node in reachableNodes {
				remove nodes not in the check scope
				
				blablabla
			}
			
			check.process(nodestocheck)
			sendmail if result
			if check as action associated
			actiion.process
		}
	}

}




comon::

GTS.commonlib
:extension de type
:helpers (mails, constants, md5)
wmi

GTS.GRD
:configuration IOCmanager
:extension de type
:helpers (mails, constants, md5)

GTS.GRD.domain
DB: nhibernate
poco + hbm.xml

GTS.GRD.Interfaces
interfaces des services notemment

GTS.GRD;Service
nodesactions
nodechecks
(abstracts)
PersistenceManager

tout les managers: servent a récupérer des ressources comme les auditnodes, passetrelle avec la persistence































