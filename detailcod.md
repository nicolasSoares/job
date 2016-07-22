RUN(){
	boucle infini tout les x temps
		->PROCESS()
}

PROCESS(){

	foreach config_clusters {

		list curClusters = (include - exclude) of currentcluster
		foreach curClusters {
			if cluster is dead -> SENDMAIL, continue
			
			/****  test if nodes are reachable */
			list nodesOfcluster = symphonyClusters of curCluster
			foreach nodesOfcluster {
				if node is in maintenance or unavailable or unreachable -> continue
				
				ping le node
				if ping success and wmi connectivity is ok -> add node to reachableNodes
			}
			
			ajoute curCluster dans actionsDone
			
			CATCH :=> SENDMAIL
			
			foreach checks of currentcluster {
				(tout les check sont ajoutés dans actionsdones[ currentcluster ] si ils n'y sont pas deja)
			
				foreach node in reachablenodes {
					filter remove check if not in checkscope
					
					if node is not in actiondone[currentcluster][currentcheck] {
						if no simphony for the check in config {
							if lim work -> add check to nodestocheck
							else may will remove node from reachableNodes
						} else -> add node to nodestocheck
					} else {
						if node.timeToCheck is ok (from actionsdones){
							remove node from actiondone
							add node to nodestocheck
						}
					}
				}
				
				remove all nodes from reachableNodeToremove
				
****			msg = check.PROCESSCHECK // modifie string msg List<node> nodestoact (and maybe list nodestocheck)

				add all nodes in nodestoact (got from check execution) to actionsDone[currentcluster][currentCheck]
				
				if currentCheck has an action assotiated { //l 543
					get action (we will call it currentaction from now on)
					
**					action.PROCESSACTION
					(amay append stuff to msg)
				}
				
				if (msg) {
					(may append to msg)

					SENDMAIL
				}
				
				CATCH :> SENDMAIL
			}
		}
	}

}






