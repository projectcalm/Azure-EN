# Tenancy Model


Fuzzy Concept of sharing in cloud applications


SaaS vs Tenancy
	Buy application vs buy service
	Is the customer the end user?


Tenants vs Instances

Models
Multi-instance
	Isolation Levels

Single instance
	Shared	


Software as service

Factors
	Automation
		High to	low — SaaS, shared, isolated, independent
	Flexibility
		High to	low — independent, isolated, shared, SaaS
	Capacity
		High to	low — SaaS, shared, isolated, independent
	Economics
		High to	low — SaaS, shared, isolated, independent


Software as Service
	No tenants
	Key activities
		On boarding
		Provisioning
	Service based pricing model
		Billing unit from Azure vs billing unit to customer
		Business oriented billing units - e.g. number of times item is scanned (versus 'seats')
			Consumption
			Transactions
			Seats
	Psychology of the buying decision
		Fixed costs vs variable costs
		Opt out and ability to change

Self service		 

User awareness of multi tenancy
Customer branding
Maximise resource utilisation
Automated on-boarding
Data isolation
	Separate servers, separate databases, schema isolation, row isolation
	Influence on cloud principles in data isolation
	Default pattern
	Row isolation — issues with backup and restore
	Shared connections (and connection strings). SQL Azure throttles if connections are left open
Customer isolation - on-boarding, upgrades, backups

Tenant identifiers
	Login
	URL
	Subdomain
	Domain name

http://cloudstrategyblog.com/2012/03/30/single-tenant-multi-tenant-saas-invoicing-money-with-the-cloud/

http://cloudstrategyblog.com/2012/02/25/cloud-charging-model/

http://cloudstrategyblog.com/2012/03/30/single-tenant-multi-tenant-saas-invoicing-money-with-the-cloud/

http://www.businesscloud9.com/files/siftmedia-bc9/u11/fujitsu.png

http://www.businesscloud9.com/content/identifying-tenant-multi-tenant-azure-applications-part-1/10242

http://blogs.msdn.com/b/usisvde/archive/2012/04/30/multi-tenant-metering-for-windows-azure-available-on-codeplex.aspx

http://www.davidchappell.com/writing/white_papers/How_SaaS_Changes_an_ISVs_Business--Chappell_v1.0.pdf