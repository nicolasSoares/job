

if _onlyCheckNotClosedServers || _onlyCheckClosedServers { ==> BOTH

	for Node node in nodes {

		if (iServerInMaintenanceMgr.isInServerInMaintenance(node.Name)) {

			for ServerInMaintenance s in sList { ==> in serverInmaintenance_OPEN

				if (s.Comment.Equals(Reason)) { ==> si y'a une raison
					ISCLOSED
				}
				else {
					ISOPEN
				}

			}

		}
		
		if ISCLOSED && _oCclo    ||     ISOPEN && _oNOTclo { ==> ISCLOSED && MAXMODE  ||  ISOPEN && MINMODE
		
			nodesNew.Addnode
		
		}
	}
	nodes = nodesNew;
}

foreach (Node node in nodes) { ==> 

	foreach (WMI.LogicalDisk ld in ldc) {
	
		if (_drives.Contains("ALL") || _drives.Contains(ld.Name.Substring(0, 1))) {
		
			 nbDrives++;
			if space <= _threshold && MINMODE  ||  space > _threshold && MAXMODE { ==> si la mem va pas ==> PROBLEM
				nbDrivesAlertTriggered++;
			}
		}
	}
	
	if MAXMODE && ALL_driveTRIGER  ||  MINMODE && ANY_driveTRIGER { ==> CONSEQUENCE -> MIN->CLOSE  MAX->OPEN
					nodetoact ADD if notcontain
	}
}

return



ouvre si tout va pas
ferme si va pas




-------------------------------------------------------------------------------------------------------------
DISKSPACE 

foreach (Node node in nodes) { ==> 

	foreach (WMI.LogicalDisk ld in ldc) {
	
		if (_drives.Contains("ALL") || _drives.Contains(ld.Name.Substring(0, 1))) {
		
			 nbDrives++;
			if space <= _threshold {
				nbDrivesAlertRaisedTriggered++;
			}
			if space > _threshold {
				nbDrivesAlertClosedTriggered++;
			}
		}
	}
	
	mynode = requestNodeInCompliance(node) -> get in gridcompilance if it exist the last entry for this node with resolvedDate == NULL
	if ANY_driveTRIGER with nbDrivesAlertRaisedTriggered {
		nodetoactraised ADD
		
		if mynode == NULL {
			pushtoDB(node)
		} else {
			maybe: updateDB
		}
	}
	
	if ALL_driveTRIGER with nbDrivesAlertClosedTriggered{
		nodetoactclosed ADD
		if mynode != NULL {
			updateDB(node)=> set resolveddate
		}
	}
}

-------------------------------------------------------------------------------------------------------------
egohgosttype


foreach (var n in nodes)
            {
                if (n.AuditNodeDetails!=null && n.HostType!=null && n.HostType.Length>0 )
                {
				
						mynode = requestNodeInCompliance(node) -> get in gridcompilance if it exist the last entry for this node with resolvedDate == NULL

                    if ((n.AuditNodeDetails.OsArchi == 32 && !(n.HostType.Equals("NTX86")))
                        ||
                        (n.AuditNodeDetails.OsArchi == 64 && !(n.HostType.Equals("NTX64")))
                        )
                    {
                        res.AppendLine("Host " + n.Name + " is " + n.HostType + " according to EGO but is " + n.AuditNodeDetails.OsArchi + " bits according to GridAudit");
                        nodesToAct.Add(n);
						
								if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}

                    }
					else {
						if mynode != NULL {
							updateDB(node)=> set resolveddate
						}
					}

                }
            }

-------------------------------------------------------------------------------------------------------------
EGVENTLOGENTRYOCCURENCE

						mynode = requestNodeInCompliance(node) -> get in gridcompilance if it exist the last entry for this node with resolvedDate == NULL

                    if (null != moc && moc.Count > _threshold)
                    {
                        resultMsg += node.Name + "\t" + String.Format("{0} occurence(s)", moc.Count) + Environment.NewLine;

                        if (!nodesToAct.Contains(node))
                            nodesToAct.Add(node);
														if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}

                    }
					else {
						if mynode != NULL {
							updateDB(node)=> set resolveddate
						}
					}


-------------------------------------------------------------------------------------------------------------
FILESPACE

						mynode = requestNodeInCompliance(node) -> get in gridcompilance if it exist the last entry for this node with resolvedDate == NULL

                if (!checkReturn.IsNoE())
                {
								if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}

				}
					else {
						if mynode != NULL {
							updateDB(node)=> set resolveddate
						}
					}


-------------------------------------------------------------------------------------------------------------
NODERESATRIBUTE

foreach (var n in nodes) {

	if (n.AuditNodeDetails != null) {

		if ((n.AuditNodeDetails.Location != null) && (n.ResourceAttribute != null)) {
			mynode = requestNodeInCompliance(node) -> get in gridcompilance if it exist the last entry for this node with resolvedDate == NULL
			if (......BULL) {
								if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}
			}
					else {
						if mynode != NULL {
							updateDB(node)=> set resolveddate
						}
					}
			if (......HP) {
								if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}
			}
					else {
						if mynode != NULL {
							updateDB(node)=> set resolveddate
						}
					}

		}

	}

}

-------------------------------------------------------------------------------------------------------------
SERVICESTATE

			mynode = requestNodeInCompliance(node) -> get in gridcompilance if it exist the last entry for this node with resolvedDate == NULL

if (sc.Count == 0 && _alertIfNotExist) {
								if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}
}
			else {
				if mynode != NULL {
					updateDB(node)=> set resolveddate
				}
			}


			
			
foreach (WMI.Service s in sc) {

	if (s.State.ToUpper().Equals(State.ToUpper())) {

										if mynode == NULL {
									pushtoDB(node)
								} else {
									maybe: updateDB
								}

	}
				else {
				if mynode != NULL {
					updateDB(node)=> set resolveddate
				}
			}

}




-------------------------------------------------------------------------------------------------------------
SYMPHONYFILEREFERENCE



if (currentRefFile.Exists) { ===> THERE

	for (Node node in nodes) {
	
		fc.Process(node.Name, out filesSetResults, true);
		
		for (string filesSetName in filesSetResults.Keys) {

			if (m.Success) {

				

			} else {

				

			}

		}
		
		if (symphonyVersionsNodes.ContainsKey(symphonyVersionId)) {


		} else {


		}
	
	}
	
	for (int symphonyVersionId in symphonyVersionsNodes.Keys) {

		if (symphonyVersionId > 0 && symphonyVersionsNodes[symphonyVersionId].Value.Count == nodes.Count) {
		}
	
	}

	if (!commonVersionFound) {

		if (symphonyVersionsNodes.Count == 0) {
		} else {
		
			for (int symphonyVersionId in symphonyVersionsNodes.Keys) {
				
				if (symphonyVersionsNodes[symphonyVersionId].Value.Count > 0) {

					for (Node symphonyVersionNode in symphonyVersionsNodes[symphonyVersionId].Value) {

						if (DisableRemediationConfigGeneration == false && version == _UndefinedSymVersion) {

							for (KeyValuePair<string, List<Node>> missingConf in missingConfigs) {
							
								if (new FilesChecker(missingConf.Key).IsServerFilesMatchRef(symphonyVersionNode.Name) == true) {
								}
							
							}

						}


					}
				
				}
				
			}
		
		}

	}
	
} else {



}



























