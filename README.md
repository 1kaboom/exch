o	Managing 7200+ mailbox across the organization access via OWA, OUTLOOK, Active sync mobile devices. 


Exchange Organization Configuration (Address Lists, Transport Rules, Mailbox Databases). 
https://practical365.com/exchange-server/tell-transport-rule-applied-email-message/



120 active databases, and 360 passive copies (including the lagged copy) 

mailbox restore and database restore

o	Exchange Servers Configuration (Certificate, Virtual Directories, URL, Connectors).


o	Exchange Recipient Administration (Creating Users, Groups, Contacts).


o	Managing Enterprise Vault Archiving solution (Veritas) for Journaling /Archiving Emails. 



o	Maintain enterprise-level configurations and management systems for Microsoft Exchange
https://www.alitajran.com/install-cumulative-update-exchange-2016/
https://practical365.com/exchange-server/installing-cumulative-updates-on-exchange-server-2016/


complex technical issues related to Exchange services


scripting: PowerShell to delete bulk emails.


o	Stop automatic replies, email access issues, Meeting Room set up, Calendar permission issues 

o	Routing emails to a different email account, Scan issue, Increase limit of recipients


o	Merge old mailbox with the new account, Email connectivity issue from Mobile devices



o	Put mailboxes legal holds



•	Handling all Audit & Management requests to provide extensive reports. 


•	Provide support on ADFS, WAP in regards to the Messaging solution.



•	Provide end to end support for mail flow (Internal & External).




•	Provide support on MS Exchange Online -Hybrid setup. The final phase of the setup of MS Exchange Online O365.


ii.	Start moving some of the mailboxes to Exchange Online.





++++++++++++++++++++++++Misc Exchange +++++++++++++++++++++++++++++

*** Services resposible for Exchange Server DBs:
	. MSExchangeADTopology 
	. MSExchangeIS
	. MSExchangeRepl
	. MSExchangeMailboxAssistants
	. MSExhangeDagMgmt

----------------------------------------
Creating and Managing Mailbox Databases [https://youtu.be/JVHKk69q6fE?list=PLimvm-6AkZMbS3ZT8NYlMRyuGzcc2i9Yi&t=239]
set-mailboxdatabase -id [dbname] -deleteditemretention 20.00.00:00 -circularloggingenabled $true -prohibitsendquota 2.2gb

dismount-database -id DBname

mount-database -id DBname

-----------------------------------------------
Managing Mailbox Settings

enable-mailbox -id username -database "dbname"

get-user -organizationalunit OUname | enable-mailbox -database "dbname"

-----------------------------------------------
Managing Public Folder Mailboxes

get-publicfolder -recurse | fl

new-publicfolder -name Research -path \Departments -mailbox mbxname

-----------------------------------------------
Configuring Address Book Policies (for differnet department)

1) new-globaladdresslist -name "NameOfGAL" -recipientfilter {{department -eq "NameofDepart"}}
update-globaladdresslist -ID NameOfGAL 
2) new-offlineaddressbook -name "NameOfGALOAB" -addresslists "NameofGAL"
3) new-addressbookpolicy -name "NameOfGALABP" -addresslists \rootfolder\NameofGAL -offlineaddressbook NameOfGALOAB -Globaladdresslist NameOfGAL -roomlist "Allrooms"
4) Get-mailbox -organizationalunit OUname | set-mailbox -addressbookpolicy NameOfGALABP

-----------------------------------------------
 Configuring Email Address Policies


-----------------------------------------------
Managing Exchange Recipients By Using Exchange Management Shell
https://youtu.be/NBF8R9QEiVU?list=PLimvm-6AkZMbS3ZT8NYlMRyuGzcc2i9Yi&t=117
. new-mailbox 
. get-mailbox
. set-mailbox
. set-user
 
. get-user -recipienttypedetails User -filter {department -eq "depart-name"} | enable-mailbox 
. New-distrbutiongroup -name "Distgrp name" -displayname "disp name" -alias 'name alias' organizationalunit 'adatum.com/ouname'

. get-user -filter {department -eq "depart-name"} | Add-distrbutiongroupmember 'Distgrp name'
. Get-distrbutiongroupmember 'Distgrp name' | measure-object


-----------------------------------------------
Managing Server Configuration With Exchange ManagementShell

. New-mailboxdatabase -server srvName -Name DbName
. Get-mailboxdatabase DbName | fl Name,ProhibitReceiveQuota
. Set-mailboxdatabase DbName 
. Get-mailboxdatabaseCopyStatus -server SrvName 
. Mount-database 'DbName'
. Restart-service MSExchangeIS 

. get-user -recipienttypedetails UserMailbox -filter {department -eq "depart-name"} | new-moverequest -targetDatabase "TargetDB" 

. get-moverequest | group "Status"

Mailbox location
. Get-MailboxDatabase | fl Name,EdbFilePath,LogFolderPath

-----------------------------------------------
 Monitoring Exchange Servers By Using Exchange Management Shell
. test-replicationhealth
. test-servicehealth
. send-mailmessage -smtpserver "srvName" -port 25 -from 'abd@123.com' -to 'noreply@newdomain.com' -subject 'test' -body 'test 123'  

email message was not received check the transport queue ::
. get-queue -server ExSerName
. Get-Queue | Get-Message | fl
. get-queue -server ExSerName\unreachable | get-message | fl
. get-queue -server ExSerName\unreachable | get-message | remove-message -confirm:$false

. Get-EventLog Application -Newest 10 -ComputerName compName


-----------------------------------------------
Using An Exchange Management Script
https://youtu.be/FQa-EYFdbfg?list=PLimvm-6AkZMbS3ZT8NYlMRyuGzcc2i9Yi&t=32

-----------------------------------------------
Configuring Offlice Online Server Integration
https://youtu.be/27A9zozAYlc?list=PLimvm-6AkZMbS3ZT8NYlMRyuGzcc2i9Yi&t=42

get-organizationConfig 

Set-organizationConfig -WACDiscoveryendpoint http://oos.beone.net/discovery

-----------------------------------------------
Monitoring Replication Health Of A DAG
. test-replicationhealth
. Get-mailboxdatabaseCopyStatus -server SrvName 
. rootpath\Exchange server\V15\scripts> .\checkdatabaseredundancy.ps1 -mailboxdatabasename "mdbName1"

. get-mailboxstatistics -database DBname

-----------------------------------------------
Configuring Transport Settings

. get-transportconfig
. Set-transportconfig
. get-transportconfig | fl *max*




-----------------------------------------------
Configuring Accepted And Remote Domains????

. get-remotedomain | fl
. new-remotedomain -name contoso -domainname contso.com
. Set-remotedomain ...


-----------------------------------------------
Configuring SMTP Send and Receive Connectors??
. new-sendconnector -name "send to internet" -addressSpaces * -sourceTransportservers srvEx1,srvEx2


-----------------------------------------------
Configuring And Using Transport Rules???


Configuring And Using A DLP Policy

Configuring Edge Sync

Configuring AntiSpam Features In Exchange Server 2016 [https://youtu.be/LOHHd7li9v8?list=PLimvm-6AkZMbS3ZT8NYlMRyuGzcc2i9Yi&t=193]
. rootpath\Exchange server\V15\scripts> .\install-antispamagents.ps1
. restart-service MSExchangeTransport 
. set-transportconfig -internalSMTPServers @(add="x.x.x.a","x.x.x.b")
. get-trasportagent
. get-contentfilteringconfig | fl enabled
. add-contentfilterPhrase -influence badword -phrase "poker results"
. add-contentfilterPhrase -influence goodword -phrase "Report docs "
. get-contentfilterPhrase 


Configuring Audit Logging
. get-adminauditlogconfig
. search-adminAuditlog -cmdlets Add-Adpermission
. set-mailbox -id "user name" -auditDelegate sendAs,sendonbehalf - Auditenabled $true
-----------------------------------------------
Migrate Mailbox Between Database by Command

. get-mailbox -id emailadd@asd.com | select database [check mailbox DB reside]
. New-MoveRequest -Identity agruber@contoso.com -TargetDatabase "MBX 02" -BadItemLimit 100 -priority Emergency [-ArchiveTargetDatabase "MBX 03" ]
. Get-MoveRequest | Get-MoveRequestStatistics


Arbitration/System Mailbox

. get-mailbox -arbitration | fl name,displayName,ServerName,Database,AdminDisplayVersion

Discovery Mailbox
. get-mailbox -resultsize unlimited -filter "RecipientTypeDetails -eq 'DiscoveryMailbox'"

Mailbox Size
. get-mailboxstatistics 'username' | select-object -property DisplayName,TotalItemSize,ItemCount

Showing data for all users [Mailbox Size]
. Get-MailboxStatistics | where {$_.ObjectClass –eq “Mailbox”} | Sort-Object TotalItemSize –Descending | ft @{label=”User”;expression={$_.DisplayName}},@{label=”Total Size (MB)”;expression={$_.TotalItemSize.Value.ToMB()}},@{label=”Items”;expression={$_.ItemCount}},@{label=”Storage Limit”;expression={$_.StorageLimitStatus}} -auto


certificates to Exchange Server 
. Get-ExchangeCertificate | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,Services

find the thumbprint value of the certificate that you want to renew:
. Get-ExchangeCertificate | where {$_.Status -eq "Valid" -and $_.IsSelfSigned -eq $false} | Format-List FriendlyName,Subject,CertificateDomains,Thumbprint,NotBefore,NotAfter


-----------------------------------------------
troubleshoot database availability groups
. Test-ReplicationHealth
. Get-DatabaseAvailabilityGroup | select -ExpandProperty:Servers | Test-ReplicationHealth
. Get-DatabaseAvailabilityGroup | Select -ExpandProperty:Servers | Test-ReplicationHealth | Where {$_.Result.Valu
e -ne "Passed"}
. Test-ReplicationHealth | Format-List Check*


Verify Required Services are Running
. Test-ServiceHealth -Server E15MB3
. Invoke-Command -ComputerName E15MB3 {Start-Service W3Svc}


Verify End to End Mail Delivery
. Test-Mailflow 
. Test-Mailflow E15MB1 -TargetMailboxServer E15MB2 [Test mail flow between two Mailbox servers]
. Test-Mailflow E15MB1 -TargetDatabase "Mailbox Database 2" [test specify database]
. Test-Mailflow E15MB1 -TargetEmailAddress paul.cunningham@exchange2013demo.com [testing mail flow to an email address]


Verify Exchange ActiveSync
. Test-ActiveSyncConnectivity -ClientAccessServer E15MB2 [test a remote Client Access server]
. Test-ActiveSyncConnectivity -URL https://mail.exchange2013demo.com/Microsoft-Server-ActiveSync [specific URL]
. Test-ActiveSyncConnectivity -TrustAnySSLCertificate 
. Get-ClientAccessServer | Test-ActiveSyncConnectivity


Verify Web Services Functionality: https://practical365.com/exchange-server/exchange-2013-test-outlook-web-service/
. Test-OutlookWebServices
. Get-ClientAccessServer | Test-OutlookWebServices -Identity paul.cunningham@exchange2013demo.com -MailboxCredential (Get-Credential)


Verify the Mailbox Replication Service
. Test-MRSHealth
. Get-MailboxServer | Test-MRSHealth | Select Identity,Check,Passed,Message | ft -auto

 test your MX record
. Resolve-DnsName -Type MX exchange2016demo.com
. MXToolbox.com 
. https://testconnectivity.microsoft.com/
. https://toolbox.googleapps.com/apps/messageheader/

.https://practical365.com/exchange-server/exchange-server-protocol-logging/
.https://practical365.com/exchange-server/exchange-transport-server-back-pressure/ 


Testing the Send Connector
. https://testconnectivity.microsoft.com/tests/exchange

Backup exchange
. get-mailboxdatabase -status | select name,lastfull*,lastinc*, | ft -auto 

Transport Log checking
. get-transportserver | select name,messagetracking*

Message tracking log searches
. 	$servers = get-exchangeserver
	$servers | get-messagetrackinglog -sender emailadre -start (get-date).addhours(-2)

. 	$msg = $servers | get-messagetrackinglog -sender emailadre -start (get-date).addhours(-2)
	$msg | sort timestamp  | fl [complete info about msg]


-----------------------------------------------
-----------------------------------------------
-----------------------------------------------



