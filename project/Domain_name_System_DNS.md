# Domain Name System (DNS)

The Domain Name System (DNS) is a hierarchical and distributed naming system for computers, services, and other resources on the Internet or other Internet Protocol (IP) networks. It associates various information with domain names (identification strings) assigned to each of the associated entities. Most prominently, it translates readily memorized domain names to the numerical IP addresses needed for locating and identifying computer services and devices with the underlying network protocols.

When a user enters a URL in their web browser, DNS gets to work to connect that URL to the IP address of the actual server. This is called DNS name resolution and involves a DNS recursor querying various name servers to figure out the actual IP address of a server.

DNS is primarily concerned with four components:

*	Domain Registrar
*	Name Servers
*	DNS Records
*	Web-based services (such as website hosting and email)
  
DNS records associate a domain with a web-based service.

There are several different types of DNS records, but in most cases, only 4 or 5 types of DNS records are used:

*	A records: Used to point a domain or a subdomain at an IPv4 address. This is the rule used to point a domain like example.com to the web server where the example.com website lives. If a web server uses an IPv6 address rather than an IPv4 address, then an AAAA record is used rather than an A record.
*	CNAME records: Used to associate a subdomain to the primary or canonical domain. This type of rule is commonly used to associate a www subdomain with the primary domain, such as www.example.com with example.com.
*	MX records: Used to associate a domain with an email service. This is the type of rule used if you want mail for example.com to be delivered to a specific email service such as Gmail.
*	TXT records: Used to associate any arbitrary text to a domain. Most commonly, TXT records are used to associate SPF records with a domain to improve email deliverability and protect against spammers misusing the domain name when sending out spam. Check out our in-depth blog post on email authentication and why it’s important. 

Other types of DNS records that are less common include:

*	CERT records: Provides a space in the DNS for certificates and related certificate revocation lists (CRLs).
*	NAPTR records: This stands for “Naming Authority Pointer.” It is a type of DNS record that maps domain names to URIs and other records.
*	PTR records: “Pointer records” are used for the Reverse DNS lookup.
  
## DNS configuration 
For this task we will define DNS service settings on your control server WindowsServer_DC. In the assignments, we will get to know the operating logic of the DNS system as well as the DNS zones and the records inside them.
The role of the DNS service in our domain is very important, because without it other machines in your domain cannot join our domain and name resolution does not work on computers that only use the DNS service of our control server.
For the first task we will be making small changes to the DNS forwarders used by our DNS service. The DNS service was automatically installed on our WindowsServer_DC virtual machine by promoting it to the control server role in the exercise at the beginning of the course. First, we login to our WindowsServer_DC and WindowsServer_FileServer.
In the server manager of our WindowsServer_DC, select DNS from the Tools menu. In the DNS manager window select Forwarders and choose properties. From the opened Properties window, open the Forwarders tab and select Edit. We added a new DNS name server as a new Forwarder here as requested by task.
 
Figure 1. DNS Forwarder Configuration
For the second task we examine the role of the DNS cache as part of the use of networks, as well as what different DNS data are stored by computers in different DNS roles. 
DNS cache refers to a temporary storage mechanism used by devices and applications to store previously resolved domain name lookups. When you visit a website or connect to a server using its domain name (e.g., www.example.com), your device typically needs to resolve that domain name to an IP address (e.g., 192.0.2.1) using DNS servers.
DNS caching helps improve the efficiency of this process by storing the results of previous DNS lookups locally. When you access a website or service again, instead of querying a DNS server to resolve the domain name, your device first checks its local DNS cache. If the domain name is found in the cache and hasn't expired, the associated IP address is retrieved from the cache, saving time, and reducing network traffic.
DNS cache entries have a time-to-live (TTL) associated with them, which specifies how long the resolved information can be stored before it expires. Once the TTL expires, the cached DNS entry is considered stale, and a new DNS lookup is required to refresh the cache with updated information.
Addition of records and reverse zones
For this task we create two alias names or CNAME records for the Forward zone of our domain, which refer to the name of our file server. Alias names are references to some A (Ip address) records. Alias names are useful if, for example, we want to refer to a server with a simplified (different) name. The alias names to be created are as follows:
•	web.ghimire.lan 
•	intra.ghimire.lan
Open DNS Manager and further view to: Forward Lookup Zones, by mouse right click in the zone area, and select from quick menu New Alias (CNAME). Here a new window is opened where we provide the details as requested by our task.
Reverse Zone
Reverse Lookup Zones are mainly used when you want to find out the name information corresponding to an IP address. For configuration purposes, we create a new reverse lookup for our domain, and add the necessary PTR records (Pointer record) inside it.
In the DNS Manager, click the right mouse button Reverse Lookup Zones and choose New Zone. A new wizard opens where we provide details as per task. You can see in the DNS Manager window of your control server that the reverse zone 0.208.10.in-addr.arpa has been created. It includes SOA and NS records by default. 
Here is the summary of settings.
 
Figure 2. New reverse zone configuration summary.

Next, we add the PTR records for our control server and file server to our new reverse zone. Your new reverse zone 0.208.10.in-addr.arpa is the body for adding PTR records.
In the DNS manager window, From the tree view, select the previously created reverse zone 0.208.10.in-addr.arpa. In the reverse zone record view, right-click and select New Pointer (PTR), Where we provide details as per task.
After the process CNAME records for your domain zone, New Reverse Zone 0.208.10.in-addr.arpa and PTR records for your reverse zone are added. So, our final result looks as follows.
 
Figure 3. Checking forward zone using nslookup

By using the above command, it returns the corresponding IPv4 address of the host name.
 
Figure 4.Checking reverse zone using nslookup.

By using these commands, it performs a reverse DNS lookup to find the associated hostname (PTR record) for that IP address.
Adding arbitrary zones to the DNS service
New zones can be arbitrarily added to the DNS name service without the computers referenced in them even existing. In this task we will configure an arbitrary network space 10.120.41.0/24, and completely made-up hostnames. At the end of the task, we will test that our file server WindowsServer_FileServer finds references to the records of arbitrary zones created with the nslookup command.
By applying the same instruction of creating reverse zone above we first create a new reverse zone in network space 10.120.41.
Next, we create a new Forward Lookup Zone. In the DNS Manager window tree view, click Forward Lookup Zones, and from quick menu New Zone. In the wizard we provide details as requested by task. The summary of our Forward Zone setting is as follows.
 
Figure 5. Forward zone setting summary.
Our forward zone looks like the following image.
 
Figure 6. Final view of forward zone and reverse lookup zone.
Next, we add A (Ipv4 address) records, so the Ip address are directly referred to by the host name. We add the record by selecting new Forward zone (GHIBIS.COM) Right-click on the zone area and select the option New Host (A or AAAA). We add 3 hosts required by task as follows:
Table 1. List of hosts and Ip address
HOST-1	10.120.41.10
HOST-2	10.120.41.20
HOST -3	10.120.41.30

After successfully adding the hosts, we now have a new Forward zone, a Reverse zone, and the A and PTR records in them. Our result look as follow,
 
Figure 7. Checking Forward zone using nslookup.

Above query (Fig 7) is a Forward lookup type query because this query resolves a domain name (hostname) to an IP address. It is a forward lookup because it translates a human-readable domain name (example.com) into its corresponding IP address.
 
Figure 8. Checking reverse zone using nslookup.
Above command (Fig 8) is a reverse lookup query because this query performs a reverse DNS lookup. It takes an IP address and attempts to find the associated hostname (PTR record). Reverse lookups translate IP addresses back into domain names.
