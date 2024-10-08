# DaSHLab Assignment 2024 
> ### [_Home DaSH Lab_](https://arnabkrpaul.github.io/Dashlab/index.html) | [_Assignment 2024_](https://docs.google.com/document/d/1oK0p87q-WvWZB3XpIarPaVZF3DQUhFqfxLy2yW__mEg/pub?urp=gmail_link#h.39v2ctm6mmq)
> ### _Segment Anything Model (SAM)_
> ### _Nirvan Patil (2022AAPS1250G)_
&nbsp;

# Development Assignment [(DaSH Repo)](https://github.com/DaSH-Lab-CSIS/DaSH-Lab-Assignment-2024/blob/main/DevelopmentAssignment/README.md)

## Level 1: API Calls
<small> <i>
> * Make a script that makes API calls to an **LLM API** (like Gemma, Groq). <br>
> * The script should take **input from a text file** and save the received responses in a **.json file.**
</i> </small>


.<br> 
### <b> <i> Choice of LLM API: **Groq** </b> </i>
* Extensive, **easy-to-read documentation** when compared to Gemma.
* Allows you to project data into custom shapes.
* **Faster output speeds** and low latency.


.<br>  
### <b> <i> Code Explained </b> </i>
><tiny> _Note: All lines of **input.txt** file are treated as the prompt to the LLM_. </tiny><br>
><tiny> _Note: **output.json** file overridden for each new response._ </tiny>

* `API_script.py`: _Python script to read and store input as required._
  * 1 additional **command line argument** can be passed which is used as **model_choice**
  * Reads from **input.txt** and stores output in **output.json**
    
* `API_class.py`: _Python code for **APIcall** class._
  * **LLM Parameters:** API_Key, model_name (default: "Gemma2 9B"), stop_token (default: None), <br>
    context_size (default: 1024), temperature (default: 0.5), top_p (default: 1) can be defined <br>
    by the user while instantiating an object.
  * **input_filename** and **output_filename** can be specified while calling the object.


.<br> 
### <b> <i> Theory explored </b> </i>
<details>
   <summary> <i> LLM Parameters </i> </summary>
   
   #### Why Use top_p?
   * **Diversity in Output**: By adjusting top_p, you can control the diversity of the generated text.
   * Lower top_p values make the output more focused and repetitive, while higher values increase <br>
     diversity but may introduce more randomness.
     
   ### Temperature
   * [What is Temp Doing?](https://www.youtube.com/watch?v=YjVuJjmgclU)
     * Small Temp (say 0.5) -> initial logits: [2,1,0.5] -> logits/Temp = [4,2,1] => Clearly the bigger <br>
       probability got bigger by more margin.
     * Big t (say 2) -> [2,1,0.5] -> [1,0.5,0.25] => All probabilities got closer
   * The temperature parameter in large language models (LLMs) is a key hyperparameter that controls <br> the
     randomness or creativity of the model's outputs during text generation**. It affects how the model <br> samples
     from the probability distribution of possible next tokens.
     
   ### Temp VS top_p
   * Temp -> Increases random sampling (more Temp = less random)
   * top_p -> Restricts choices of model (more top_p = more choices for sampling)
   * 
   ### Stop token
   * stop = stop token corresponding to halting text generation
     
</details>


.<br> 
### <b> <i> References for this section </b> </i>
* [Getting Started with Groq API](https://www.youtube.com/watch?v=S53BanCP14c)
* [Groq Documentation](https://console.groq.com/docs/quickstart)
* [Groq Playground](https://console.groq.com/playground)
* [JSON Output](https://github.com/groq/groq-api-cookbook/blob/main/tutorials/json-mode-social-determinants-of-health/SDOH-Json-mode.ipynb)


&nbsp;

## Level 2: Client-Server Model
<small> <i>
> * Scale up what you have implemented in Level 1 to a **client-server model**. <br>
> * Create **multiple clients** that can read input and send this input to a server. <br>
> * Server -> API call -> sends the responses back to all clients along with the original prompt. <br>
> * The clients should now write the response they receive from the server to a **.json file**. <br>
> * Having done this, create a **bash script** that launches all clients and servers. <br>
</i> </small>


.<br> 
### <b> <i> Client-Server Protocols </b> </i>
> * **WebSockets**, owing to their simplicity of implementation in Python and full-duplex, <br>
>   bidirectional communication is chosen as the protocol for developing the client-server model.
> * **Socket.io** library used.   

<details>
   <summary> <i> HTTP vs SSE vs WebSockets </i> </summary>
   
   #### **HTTP (Hypertext Transfer Protocol)**
   
   - **Type:** Request-response protocol.
   - **Communication:** The Client sends a request to the server, and the server responds. Each request is independent.
   - **Statefulness:** Stateless; each request is separate and does not maintain a persistent connection.
   - **Use Cases:** Traditional web pages, APIs, and general-purpose data retrieval.
   - **Advantages:** Simple and well-supported; suitable for most web interactions.
   - **Limitations:** Not ideal for real-time updates or bidirectional communication.
   
   #### **Server-Sent Events (SSE)** 
   
   - **Type:** One-way, server-to-client communication.
   - **Communication:** The server pushes updates to the client over a single long-lived HTTP connection.
   - **Statefulness:** State is maintained through a single connection, but the protocol is still relatively simple.
   - **Use Cases:** Real-time updates such as live notifications, feeds, or updates where only server-to-client communication is needed.
   - **Advantages:** Simple to implement, with built-in support for automatic reconnections and event handling.
   - **Limitations:** One-way communication (server-to-client only) is unsuitable for bidirectional communication.
   
   #### **WebSockets**
   
   - **Type:** Full-duplex, bidirectional communication.
   - **Communication:** Establishes a persistent connection, allowing both the client and server to send messages to each other at any time.
   - **Statefulness:** State is maintained throughout the WebSocket connection.
   - **Use Cases:** Real-time applications such as chat applications, live updates, and interactive gaming.
   - **Advantages:** Low latency, efficient for high-frequency data exchange, and supports two-way communication.
   - **Limitations:** More complex to implement and manage than HTTP and SSE; requires WebSocket support in both client and server.
     
</details>


.<br> 
### <b> <i> Code Explained </b> </i>
> Python code present in Level1+2 Code folder <br>
> Input and output files to show working in Level1+2 Input & Output folder

* `server.py`: _Python class to create a server to handle API calls._
  * Server is created at the **IP address of the local machine**.
  * Can handle **multiple clients** by utilizing multi-threading.
  * Accepts prompt (string) from a client -> Makes an API call to the LLM using `APIcall` object <br>
    -> receives LLM response in JSON format -> Adds a "_SERVER" authentication tag to the LLM response <br>
    -> encodes LLM response and sends it to the client as requested, along with a unique client ID.
    
* `client.py`: _Python class to create clients that send and receive data to/from the server._
  * Each client is connected to the above-created server.
  * Encodes a prompt (string) and sends it to the server -> ... server ... -> Receives LLM response (string) and decodes it <br>
    -> authenticates if the prompt-response pair sent by the server -> converts LLM response to .json dict. and stores it in <br>
    output_ID.json.

 * `main.py`: _Python script to run codes for Level1 and Level2_
   * **Level 1:** Reads input from intput1.txt and stores .json response dict in output1.txt.
   * **Level 2:** Reads input from input2.txt -> Each line in input2.txt is sent as a prompt to a unique client -> Each client <br>
   stores .json response dict in `output_<CLIENT_ID>.json` e.g. `output_53525.json`.

 * `Level2.bat`: _Batch file to automate Level2 Task_
   * **Automates** the entire Level2 process from reading input to making output.json files.
   * **Batch** file used as bash file caused client-server connection to fail.
 
.<br> 
### <b> <i> Theory explored </b> </i>

<details>
   <summary> <i> Bash Script, Batch File </i> </summary>

   ### Bash Script
   - More powerful than Batch Files.
   - Primarily used on Unix-like systems (Linux, macOS).
   - Can be used on Windows via tools like Git Bash, WSL (Windows Subsystem for Linux), or Cygwin.
   
   ### Batch Files
   - Simple, text-based scripts are mainly used to automate Windows environment tasks. They are easy to create and modify but limited in functionality and performance.
   - They are interpreted line by line by the command-line interpreter (e.g., cmd.exe on Windows).

   ### .EXE Files
   - .exe files can be used to create full-featured applications with graphical interfaces, complex logic, and interactions with the operating system.
   - They can be run directly by the operating system without an interpreter.
   
 &nbsp;
</details>

<details>
   <summary> <i> Client and Server </i> </summary>
   
   - **Client:** A machine or a program used to make requests through the web.
     
   - **Server:** It is a program that listens to clients' requests and responds to them.
     
   - **Client Server Model:** Centralized web architecture where the server acts as a central hub that manages and provides resources or services to multiple clients
     
   - **Peer-to-Peer Model:** No central server; instead, each node can act as both a client and a server, distributing the responsibilities and reducing centralization.

&nbsp;
</details>

<details>
   <summary> <i> Port VS IP Address </i> </summary>

   ### IP Address
   - **What it is**: An IP address is a unique identifier assigned to a device on a network. It enables devices to locate and communicate with each other over the Internet or a local network.
   - **Types**: There are two main types of IP addresses:
     - **IPv4**: A 32-bit address written as four decimal numbers separated by periods (e.g., 192.168.1.1).
     - **IPv6**: A 128-bit address, written as eight groups of four hexadecimal digits separated by colons (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334).
   - **Function**: The IP address serves as the "address" of a device on the network, much like a street address for a house.

   
   ### Port
   - **What it is**: A port is a numerical value (ranging from 0 to 65535) used to identify a specific process or service running on a device. It allows multiple services to run on a single IP address 
                     simultaneously.
   - **Types**:
     - **Well-known ports**: Ports 0 to 1023 are reserved for systems or well-known services (e.g., HTTP uses port 80, HTTPS uses port 443).
     - **Registered ports**: Ports 1024 to 49151 are used by user-registered services.
     - **Dynamic or private ports**: Ports 49152 to 65535 are used by applications for temporary communication.
   - **Function**: The port number, along with the IP address, directs traffic to the appropriate application or service on a device. For example, when you visit a website, your browser uses port 80 (HTTP) or 
     443 (HTTPS) to connect to the web server's IP address.

   
   ### How They Work Together
   - When you access a service on a network, your device sends data to an IP address at a specific port number. The IP address ensures the data reaches the correct device, and the port ensures it reaches the 
     correct application or service on that device. 
   
   For example, when you type `http://example.com` in your browser:
   - Your request has been sent to the IP address `example.com`.
   - The request uses port 80 (the default for HTTP).
   - The web server at that IP address receives the request on port 80 and responds with the webpage.
   - **Endpoint:**
      - An endpoint combines an IP address and a port number. It specifies a particular service on a host machine.
      - Example: 192.168.1.1:8080 is an endpoint where 192.168.1.1 is the IP address, and 8080 is the port number. Together, they point to a specific service running on the host machine at that address.

&nbsp;
</details>

<details>
   <summary> <i> Domain Name System (DNS) </i> </summary>
   
   ### **Problem with IP Addresses:**
   - Every device connected to the internet is identified by a unique IP address, which is a numerical label like `192.168.1.1` for IPv4 or a more complex string like `2001:0db8:85a3:0000:0000:8a2e:0370:7334` 
     for IPv6.
   - However, remembering these numbers is not practical for most people. Imagine trying to remember the IP address for every website you visit!
   
   ### **Domain Names:**
   - To make things easier, domain names were introduced. Domain names are human-readable addresses like `www.google.com`, which are easier to remember and use.
   - Domain names are hierarchical and usually consist of a top-level domain (TLD) like `.com`, `.org`, or `.edu`, followed by a second-level domain (like `google` in `google.com`).
   
   ### **DNS as a Solution:**
   - The Domain Name System (DNS) acts as a translator or a phonebook for the internet.
   - When you type a domain name into your web browser, your computer requests a DNS server to find the IP address associated with that domain name.
   
   ### **How DNS Works:**
   - **DNS Query:** When you enter a domain name (e.g., `www.example.com`), your computer first checks its local DNS cache to see if it already knows the IP address. If not, it sends a DNS query to a DNS 
      resolver (usually provided by your ISP).
   - **DNS Resolver:** The resolver checks its cache. If it doesn't find the answer, it starts hierarchically querying other DNS servers, starting from the root DNS servers.
   - **Root, TLD, and Authoritative DNS Servers:**
     - **Root Servers:** Direct the resolver to the appropriate top-level domain (TLD) server.
     - **TLD Servers:** These direct the resolver to the authoritative DNS server for the specific domain (e.g., `example.com`).
     - **Authoritative DNS Server:** This server provides the actual IP address associated with the domain name.
   - **IP Address Returned:** Once the IP address is found, it's sent back to your computer, and your browser can then connect to the website using that IP address.
   
   ### **Dynamic IP Addresses:**
   - IP addresses can change, especially with dynamic IPs used by most ISPs. The DNS system is designed to handle this by allowing domain owners to update their DNS records with new IP addresses, ensuring 
     that users can still reach their websites.
   
</details>


.<br> 
### <b> <i> References for this section </i> </b>
* [Domain Name System (DNS)](https://www.geeksforgeeks.org/domain-name-system-dns-in-application-layer/)
* [Client-Server Model 1](https://www.geeksforgeeks.org/client-server-model/)
* [Client-Server Model 2](https://youtu.be/L5BlpPU_muY)
* [Web Sockets](https://www.youtube.com/watch?v=favi7avxIag)<br>
* [Socket.io](https://www.youtube.com/watch?v=3QiPPX-KeSc&t=114s)



&nbsp;
## Level 3: Dockerization
> **Dockerize** the system created in level 2. <br>
> Make **separate** images for servers and clients. <br>
> Use a **docker-compose** file to start the server and clients.

.<br> 
### <b> <i> Code Explained </b> </i>
> Code to create a docker image for the server and client-side code
> Docker-compose to run server and client containers
> Shared volume space for output.json

* `server_v2.py`: _Small tweaks to `server.py`._
  * Server IP is set to `self.SERVER = 0.0.0.0`
    * A **special IP address**, the server will accept **connections from any IP address**, not just a specific one.
    * self.SERVER = "127.0.0.1", the server will only accept connections coming from that IP address.

* `Dockerfile`: _Code for Docker Image_
  * Used to create a **docker image** with required dependencies, specified in requirements.txt
  * Working directory for containers /app
  * **Exposed Port** for containers- 80

* `docker-compose.yml`: _Facilitating multiple container communication_
  * Maps `host port 5050` to the server container's `exposed port 80`.
  * Crucially defines a SERVER_ADDRESS environment variable.
    * By setting **SERVER_ADDRESS** to the `server`, you’re instructing the client to use the <br>
      Docker network’s DNS to resolve the hostname server to the IP address of the server container.
  * Creates a shared volume and maps it to the client container's data space as `/data`.

* `client_v2.py`: _Small tweaks to `server.py`._
  * Client connects to IP set to `self.SERVER = "server"`
  * Output files of the client are stored at `/data/`

.<br> 
### <b> <i> Theory explored </b> </i>

<details>
   <summary> <i> Containerization and Kubernetes </i> </summary>
   
   ### **Monolith Architecture:**
   * Monolithic architecture is a traditional software development approach where all the components of an application are tightly coupled and run as a single, unified unit.
   * In this architecture, the entire application is built and deployed as one large codebase. This means the application's functions, including user interface, business logic, and data access layers, are  
     contained within a single platform or executable.

   ### **Microservices:**
   * Microservices architecture is a design approach where an application comprises small, independent services communicating with each other over a network.
   * Each service is responsible for a specific functionality and can be developed, deployed, and scaled independently.

   ### **Containerization:**
   * Containerization is a lightweight form of virtualization that allows you to package an application and its dependencies into a single container.
   * This container can run consistently across different computing environments, such as development, testing, and production. Containers are isolated from each other and the underlying operating system, 
     making them portable and efficient.

   ### **Kubernetes:**
   * Kubernetes is an open-source container orchestration platform designed to automate containerized applications' deployment, scaling, and management.
  
</details>

<details>
   <summary> <i> Docker </i> </summary>
 
   ### Docker
   * Docker simplifies creating, managing, and deploying these containers, making it easier to <br>
   * develop, test, and deploy applications in a consistent and portable manner.
   
   ### Key components
   * Containers: Lightweight, isolated environments for running applications.
   * Images: Read-only **templates** are used to create containers built from Dockerfile.
   * Docker Engine: The core component that **manages containers**.
   * Docker Compose: A tool for defining and running multi-container applications using a docker-compose.yml file.
   * Docker Hub: A cloud registry for sharing and storing Docker images.
   
</details>


.<br> 
### <b> <i> References for this section </i> </b>
* [Monolithic Arch. & Microservices](https://www.youtube.com/watch?v=7IFJb-uLEaI)
* [Docker Compose {TO DO}](https://www.youtube.com/watch?v=HG6yIjZapSA&pp=ygULZG9ja2VyIG1vc2g%3D)
* [Docker - 1hr full {TO DO}](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=155s&pp=ygULZG9ja2VyIG1vc2g%3D)
* [Docker - ApnaCollege](https://www.youtube.com/watch?v=H8Lyj2D_cWo)
* [Docker - Fireship](https://www.youtube.com/watch?v=gAkwW2tuIqE)
* [Containerization](https://www.youtube.com/watch?v=0qotVMX-J5s)
* [Kubernetes](https://www.youtube.com/watch?v=VnvRFRk_51k)



&nbsp;
# Transformers, ViT & SAM 

* [Transformers](https://arxiv.org/abs/1706.03762), [ViT](https://arxiv.org/abs/2010.11929), [Segment Anything Model](https://arxiv.org/abs/2304.02643) - `One read through complete`
* Research summary/literature review in respective .md files in the `Research` Folder - `Work in Progress`


