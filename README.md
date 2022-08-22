# SentinelOne-Threat-Hunting-Guide
Beginners Guide to Hunting for Threats

___________  ___ ___  __________ ___________   _____   ___________   ___ ___   ____ ___  _______   ___________.___  _______     ________ 
\__    ___/ /   |   \ \______   \\_   _____/  /  _  \  \__    ___/  /   |   \ |    |   \ \      \  \__    ___/|   | \      \   /  _____/ 
  |    |   /    ~    \ |       _/ |    __)_  /  /_\  \   |    |    /    ~    \|    |   / /   |   \   |    |   |   | /   |   \ /   \  ___ 
  |    |   \    Y    / |    |   \ |        \/    |    \  |    |    \    Y    /|    |  / /    |    \  |    |   |   |/    |    \\    \_\  \
  |____|    \___|_  /  |____|_  //_______  /\____|__  /  |____|     \___|_  / |______/  \____|__  /  |____|   |___|\____|__  / \______  /
                  \/          \/         \/         \/                    \/                    \/                         \/         \/ 


Threat Hunting / searching for behaviors in your environment that may be associated with malicious activity is a great way to learn more about your environments or your customers environments, so you can explore and asess what is normal and what is suspicious/malicious. This is a simple guide to show users of the XDR Platform SentinelOne how to learn some basic threat hunting concepts.


Here are the basic steps I follow when performing hunting with SentinelOne. For this guide I am going to provide a simple example: The execution of powershell where an external DNS request is made. CmdLine Contains Anycase "powershell" AND DnsRequest EXISTS

1. Hypothesis -  Determine which query or sets of queries I would like to run and what my time frame / scope is for performing a hunt. 
 
Be sure to set your Scope/Time Frame and your max results. I personally like to set my results to max 20K and time frame to 14 days to see what I get when I am running a query for the first time. THis will help detemrine how you might want to adjust your hunt once you determine how many results are returned. If your query maxes out at 20K you will want to tune it and remove false positives until you get some result less than 20K unless you determine you have 20K malicious results. Kust keep in mind that if you are maxing out at 20K results you aren't getting the full results in your query response and need to tune out false positives or adjust your time frame.
  
2. Tuning - Work to eliminate false positives from your results. If the query above returned the max of 20K results there will deifnitely be tuning required.  

The following are steps I take to tune/analyze queries.
                                          
a. Export results to CSV with all columns
b. Run a macro in excel to remove all columns that don't contian any data (Included in theis module)
c. Reformat your date columns, save your CSV as an xlsx, lock the top rows and use the data function to filter. Sort
colums of importance to group similar things together as needed.
d. Build a pivot table in excel to analyze my result. (In the example above I am interested in two 
main data sources. Data that contains comand line information and the DNSRequest information. If I detemine I have 20K dns requests to microsoft.com
I may need to adjust my query to something like: CmdLine Contains Anycase "powershell" AND DnsRequest EXISTS and DnsRequest Does Not ContainCIS "microsoft.com" 
e. Use this csv to see how data is stored in SentinelOne. In my experience things change quite 
frequently so the more you hunt the more you may notice new pieces of info that can help in your hunting.
                                          
3. Evaluate -   If I determine this is a high fidelity search for which my criteria is low false positive rate and useful to be notified on as it may indicate suspicious behavior, I would consider turning this into a STAR rule for automated threat notification.
                                          
a. For this search I would look at the command line information and domains from the results and determine 
if this is expected behavior or if it's potentially malicious. 
                                          
Command Line - simply evaluating what the source process/script is, where it's executing from and if that's 
expected behvior might be enough to let me know if I need to escalate and investigate. Using the file fetch 
capability in SentinelOne I can pull back the script or EXE and evualute it further in a malware sandbox or through other static/dynamic analysis techniques.
                                          
Domains - Create a pivot table on the domains and eliminate the known good and further analyze the 
suspicious looking domains with OSINT tools like Virus Total.
                                          
                                          
                                          
  
  
 

 
 


