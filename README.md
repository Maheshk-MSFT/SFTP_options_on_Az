# SFTP_options_on_Az
SFTP_options_on_Azure

SFTP Client login accounts are not able to discover our mounted drivers. Tried various things but could not be able to work around as it’s a protocol + Linux behavior. 

It’s able to discover normal Linux folders from SFTP client but when we mount any folders like containers using Blob Fuse or normal NFSs, then it’s not loading it. Kind note, this is nothing to do with our Azure svcs - you would see the same behavior across any cloud. 

**Options	
**SFTP server on Azure VM 	
1.	Using BlobFuse2 to mount the Azure storage Blob container	It works within the Azure VM with our local account’s login accessing the mounted container 	It does not work – validated
**From SFTP Client**
-	SFTP login account is not able to discover the Blobfuse mounted path
-	Mounting needs certain drivers to be loaded in the user space; FTP login does not have access/way to load the mount
-	It’s able to list down all other normal Linux folders  	
**Not a viable option for SFTP setup**

**2.	Mount Blob Storage by using the Network File System (NFS)**
	It works within the Azure VM with our local accounts login (ssh)	It does not work - validated
-	Same as like Blobfuse
-	We can browse all other folders except the NFS mounted ones are not shown
-	Access denied on forcible entry	
**Not a viable option for SFTP setup**
 	
**3.	Azure Blob Storage SFTP**
It works as expected from any client VM (*). 

* VM and Blob Storage must be in the same VNET

* SFTP configurations are not required	It works as expected	Viable 

-	suits well for your requirement
-	less operation headache – BCDR etc. 
-	Built in HA, Pay for usage
-	Easy to export and Import the data
-	Can automate container creation for new customer onboarding, SAS & ACL’s on the fly - who can do what etc. 
-	All this can be achieved without getting into hardcore Linux & user profile mgmt issues
![Screenshot 2024-08-27 163026](https://github.com/user-attachments/assets/af967a42-6fd9-4373-893e-a2b8f83861d7)


 https://github.com/Azure/azure-storage-fuse/blob/main/setup/baseConfig.yaml
 https://learn.microsoft.com/en-us/azure/storage/blobs/network-file-system-protocol-support-how-to
 https://learn.microsoft.com/en-us/azure/storage/blobs/secure-file-transfer-protocol-support-how-to?tabs=azure-portal
