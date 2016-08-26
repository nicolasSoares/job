Hello, my name is Nicolas soares, I'm about to finish, I hope, the epitech curiculum and to conclude those last five years I made an internship In the societe generale company that I will present to you

Société Générale is one of the main and oldest French multinational banking and financial
services company.
The company group is a universal bank divided into 3 main pillars :
- Retail banking (Société Générale, Crédit du Nord and Boursorama)
- International Banking and Financial Services (IBFS)
- Corporate and Investment Banking (SG CIB)
Those 3 pillars are backed by 2 other core activities :
- Specialised Financial Services & Insurance
- Private Banking, Global Investment Management & Services

services

	RESG : direction of the group's ressources and innovations, it gathers the teams in charge of information systems, technical infrastructures, purchases estate operations and counsel activities of the group
		direction of the group's ressources and innovations
	GTS : Global technology Service ensures the proper functioning of computer infrastructure and their resilience and counsel and assists the business lines and functionnal divisions for the implementation of their projects
	 Global technology Service
	MKT: the wholesale bank division, its goal is to ensure the proper functionning of the computer infrastructures and the client relationship with its business partners SGCIB (corporate investment banking) and GIMS (cover the private bank activities, asset management and security services)
			market
	SSB: 	this division is in charge of operations on storage and backup of the servers, they assures the administration, maintenance and backup of all the Market's infracture and handle several projects to ensure the proper functioning of the production
		server storage backup

	GRD: the grid team is in charge of handling and monitoring the grid computing platform and infrastructure for various departments of société générale.
		The grid team is the middleware between 
		grid team

			grid team = middleware itec win/unix

what is grid computing
Grid computing is the collection of heterogeneous and affordable computer ressources from multiple locations to reach a common goal requiring intensive computation by splitting this goal into small fractions spread to each node of the grid.
By this process computations that would require large amount of time and/or supercomputers can be achieved quickly and cheaply with a robust and highly scalable environment.
The project SETI@home of the university of california is an example of grid computing where anyone installing a softawre on their computer participate in the analisys of radio signal searching for sign of extraterrestrial intelligence. 

you may still wonder 
why grid computing in SG => 
the answer is a simple concept
	you win when you are the first
	on the highly competitive abnking world the ability to quickly compute financial datas like financial risks and pricing gives a meaningful advantage to be a leader on the marketplace

	therefore we canot afford to let the grid down a single second and any issue must be identified and fixed as quiclky as possible
	and that is where the porjects I worked on come in
	
GridAudit and GridSupervisor

Several tools constantly monitore the grid's server nodes and gather an enormous amount of information about them that are available through databases and API


Gridaudit, given a list of server node, requests those databases and API and also directly requests the nodes to detect and store on the grid databse inconsistencies among those differents sources of informations
gridaudit then run postchecks to detect inconsistencies and outdated datas from the check results and alerts directly a human by mail when needed

Gridsupervisor, as for him, looks for compliance errors on the server nodes (for example memory available, ping, node status), alerts when errors are detected and can also direclty act on the nodes if needed like closing a node and put it in maintenance mode

gridsupervisor and gridaudit are .NET project, my first task was to update them from .net 3.5 to 4.5 and adapt them to match the evolution of the grid's infrastructure and tools

on gridaudit I implemented the requests of various API (this branch), and several checks processing their datas
for gridsupervisor I created new compliance checks and modified the way compliance alerts where raised, instead of alerting and logging the errors only by email, I modified the whole project to store the alerts on a specific database so the alert can be monitored from the gridhome platform, a website used to help developer and administrators to manage their grids.
By doing so I significantly reduced the amount of mail by day, (%%%%) received by the grid team members, saving them precious time increasing work effectiveness

to compile, 
this internship brought me a lot
on the technical side, 
I highly improved my skills in .net development and more generally in development on a windows environment
and I discovered in detail the inner working of one of the biggest compute grid

and on the personnal side
I discovered  the work practices and environment of a massive company of the banking world
the fundamental security side of such company, security in the main word as it define everything in something as critical as a banking company
it defines the work processes and all the different hierarchie layers to ensure that no mistake or error can be propagatted to the end user


finally what I think I provided to the grid team during my internship, aside from the technical aspect, is the same as what I acquired through thoses last 5 years, through the Epitech course and my experiences in web development in startups in my previous internships and EIP. web development and startups evolves fasts and to keep up with this fast evolution I grew what i think is one of the most important skill in the IT industry, adaptability, versatility
I brought this skill with me during this internship and I will bring it my futur professional life
