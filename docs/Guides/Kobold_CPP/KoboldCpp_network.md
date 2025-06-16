# Networking
General network settings are simple and easy to use, but for those who are new to the topics, a dictionary is at the bottom.
In admin, you can enable remote changes to launcher settings, the settings here will not be changed by that to prevent loss of access

## Networking options in the launcher
- #### Port
    - This is the Communication port used by your browser to connect. 5001 is generally recommended.
- #### Host
    - Which IP address for the host should be used. Blank is "All", same with `0.0.0.0`. `127.0.0.1` is none
- #### Multiuser Mode
    - Adds a queue system so that more than 1 client will be able to make requests at the same time.
- #### Remote Tunnel
    - Uses Cloudflare to set up a simple and easy to use shareable external link.
    - Basically: it makes it public without making your whole network public.
- #### Quiet Mode
    - Do not display logs of messages and connection requests. This should be enabled if you are hosting for others
- #### NoCertify Mode (Insecure)
    - ignores the SSL Certificate and allows messages with no encryption. This will expose everything you send to every device between the client PC and the server PC.
- #### Shared Multiplayer
    - Allows multiple users to interact while also interacting with the AI.
- #### Enable WebSearch
    - Enables use of WebSearch in Kobold Lite. 
    - If also enabled in Kobold Lite, then it sends the last 300 characters to DuckDuckGo and adds the first 5 results to the message for the AI. Comparable to "Grounding" option in Google Gemini.
- #### SSL Cert
    - Certificate file used for encryption.
- #### SSL Key
    - Certificate Key file used for encryption.
- #### Password
    - The API Key used for all requests to the server.
- #### Max Req Size (MB)
    - Max size of any given single query.



## Beginner Dictionary:
IP Address: IP address is what other devices use to route traffic to your computer. You have multiple IP addresses, the Private IP is the computer identifier within your home network, the public ip is the network identifier outside your home. Kobold only cares about your Private IP

Port: Computers have 65535 different communication options to separate which program is being talked to. web traffic (your browser) uses 80, 8080, 443 by default, but can be specified to use an alternative by adding :\<port number\> to the primary URL (ie: 8.8.8.8 is dns.google.com, but 8.8.8.8:53 is the DNS port of googles dns)

SSL: Standard web encryption method to hide private traffic from your ISP and other clients on the network. While it is not perfect, it is useful for every day purposes.