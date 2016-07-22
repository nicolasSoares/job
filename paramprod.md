servicestate			name="ServiceStateGridSnapshot"		scope="MasterCandidatesNodes" 
						alert="NODE_GRIDSNAPSHOT_DOWN"		repeatNotificationDelay="86400"

SymphonyFilesReference	name="SymphonyFilesReference"		scope="All"
						alert="SYMPHONY_DIFFERENT_VERSION"	repeatNotificationDelay="259200"	ExcludeHostsWithoutLimSVC="true"

NodeResAttribute		name="NodeResAttribute"				scope="All"
						alert="EGO_RESATTR_INCONSISTENT" 	repeatNotificationDelay="259200"

EgoHostTypeCheck		name="EgoHostTypeCheck"				scope="All"
						alert="EGO_HOSTTYPE_INCONSISTENT"	repeatNotificationDelay="259200"
				
FileSpace				name="FileSpaceSoamLogs"			scope="ManagementNodes"
						alert="SOAM_LOG_TOO_BIG"			repeatNotificationDelay="86400"		ExcludeHostsWithoutLimSVC="true"
				
FileSpace				name="FileSpaceEgoLogs"				scope="ManagementNodes"
						alert="EGO_LOG_TOO_BIG"				repeatNotificationDelay="86400"		ExcludeHostsWithoutLimSVC="true"

FileSpace				name="FileSpaceEgoLogs"				scope="ManagementNodes"
						alert="WEBGUI_LOG_TOO_BIG"			repeatNotificationDelay="86400"		ExcludeHostsWithoutLimSVC="true"

FileSpace				name="FileSpaceGRDInfraLogs"		scope="ManagementNodes"
						alert="GRDTOOLS_LOG_TOO_BIG"		repeatNotificationDelay="86400"		ExcludeHostsWithoutLimSVC="true"

FileSpace				name="FileSpaceSoamPagingFiles"		scope="ManagementNodes"
						alert="SOAM_PAGING_TOO_BIG"			repeatNotificationDelay="86400"		ExcludeHostsWithoutLimSVC="true"

DiskSpace				name="DiskSpaceMBMin"				scope="ComputeNodes"				action="CloseNodeForDiskSpace"
						alert="NODE_DISK_FULL"				repeatNotificationDelay="86400"

DiskSpace				name="DiskSpaceMBMax"				scope="ComputeNodes"				action="OpenNodeForDiskSpace"
						alert="NODE_DISK_FULL_END" 			repeatNotificationDelay="0"
						
						
DiskSpace				name="DiskSpaceMBMin"				scope="ComputeNodes" actionRaised="CloseNodeForDiskSpace" actionremediated="OpenNodeForDiskSpace"
alert="NODE_DISK_FULL"				repeatNotificationDelay="86400"
