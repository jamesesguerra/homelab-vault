
- Nginx started off as a simple web-server that would listen to requests and respond appropriately.
- Once more people started using the internet, the load of a single web server became too much, which introduced using Nginx as a **load balancer** that sits in front of several servers to distribute the traffic evenly
- It can also be used as a **caching** mechanism to store commonly requested content in the proxy so that the server won't have to keep rebuilding the same files for each request.
- Finally, you can use a reverse proxy to hide the IP address of your application servers, essentially using just one server as an entrypoint in front of the app servers. This adds **security** to the web servers. 

#### Encyrpted communication
You can configure your proxy to also only accept encrypted traffic and deny all incoming requests that aren't encrypted. You also have the option to offload the HTTPS at the server, sending unencrypted traffic to the web servers. Or you can send the encrypted traffic to the web servers and let them handle the HTTPS termination.

