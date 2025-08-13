# IBM Modernized Runtime Extension for Java (MoRE) Lab

![A person standing in front of a computer screen Description
automatically generated](./images/media/image1.jpeg)**  
**

**Document edition: August 2025**


## Table of Contents

[Introduction 1](#introduction)

[About this lab 3](#about-this-lab)

[Steps for the lab tasks 4](#steps-for-the-lab-tasks)

## Introduction

This lab guide walks through the steps to conduct an IBM Modernized
Runtime Extension for Java (MoRE) lab.

MoRE introduces the WebSphere administrative model with operational,
monitoring, and configuration capabilities currently used to manage
Liberty servers. This allows Liberty servers to be managed in the same
way as existing Java 8 applications running in traditional WebSphere
environments.

The diagram below illustrates a WebSphere Application Server Network
Deployment (WAS ND) cell currently running Java 8 applications. This is
a concrete cell with multiple nodes, utilizing WAS administrative
scripting and the administrative console. When there is a need to
migrate certain applications or develop new ones on Java 17 and Liberty,
MoRE enables the integration of a managed Liberty runtime into the same
cell, as shown in the two Liberty server boxes on the left.

![A diagram of a cell AI-generated content may be
incorrect.](./images/media/image2.png)

With Liberty running Java 17 and a subset of the Jakarta Enterprise
Edition (EE) application programming interface (API), MoRE incorporates
managed Liberty servers into the same administrative model used for
WebSphere Application Server and other applications. The ability to use
the existing administrative model with new applications and Liberty
runtimes provides significant value to many current WebSphere clients.

For clients seeking to adopt Java 17 without disrupting current
operations, this option offers a practical migration path. It enables
Java 17 to run within the existing WAS ND environment without requiring
changes to application management or deployment processes. Teams can
continue using familiar tools such as the administrative console, Java
Management Extensions (JMX), and the deployment manager while
transitioning to a modern runtime.

This low-friction approach reduces training requirements, preserves
operational stability, and offers flexibility in licensing and
deployment. In summary, MoRE enables Java modernization without
requiring a complete overhaul of existing systems.

MoRE provides the capability to continue using the traditional WebSphere
Application Server (tWAS) Operational Model to manage Java 17 and Java 8
applications within the same traditional WebSphere administrative
environment.

The key feature of MoRE 1.0.0.1 is static clusters for running managed
Liberty servers in the tWAS cell; support for the Jakarta EE 10 APIs
required to run Spring 6 applications is also now available but is not
the focus of this demo.

## About this lab

In this lab, you will learn how to extend a WAS ND Cell, using the MoRE
feature pak, for managed Liberty servers to manage and run Java 17 /
Jakarta EE 10 (subset) applications using the familiar WebSphere
administrative mode and admin console.

The lab teaches you how to deploy and run an example application called
**WhereAmI** in a WebSphere cluster, fronted by an IBM HTTP server.

Included with the lab, is a new version of the **WhereAmI**
application that is built using Java 17 and Jakarta 10 EE APIs, which
are supported in the managed Liberty Servers (MoRE).

At the end of this lab, you should be able to learn how to:

  - Install WebSphere Liberty and MoRE components through IBM
    Installation Manager

  - Create a static cluster with two managed Liberty server (MoRE)
    cluster members, using the WebSphere Integrated Solutions Console
    (WAS admin console)

  - Install the new Java 17 version of the example application and
    target the WebSphere cluster, using the WAS admin console

  - Start the cluster of managed Liberty servers, using the WAS admin
    console

  - Locate and view the managed Liberty server logs

  - View the command assistance for various tasks, showing the wsadmin
    commands that can be used to perform and automate the tasks.

  - Generate and propagate the WebSphere plugin configuration for the
    IBM HTTP Server

  - From the web browser, run the new example application running Java
    17 in a WebSphere cluster, through the IBM HTTP Server

  - Update the Liberty server HTTP port and test the high availability
    of the managed Liberty servers cluster.

# Steps for the lab tasks

1.  View the installed WebSphere components in IBM Installation Manager.

    a. Click the **terminal** icon (A) to open a **terminal** window.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image3.png)

    b.  In the **terminal** window issue the following command to launch
    **IBM Installation Manager.**

        /home/techzone/IBM/InstallationManager_Group/eclipse/IBMIM

    c.  In the IBM Installation Manager page, click the **Uninstall** (A).
    DO NOT UNINSTALL ANY COMPONENTS\!

    ![A screenshot of a computer software AI-generated content may be
 incorrect.](./images/media/image4.png)

    d.  Review the installed components, and you can see the WAS ND
    9.0.5.24, IHS 9.0.5.24, WebSphere Customization Toolbox 9.0.5.24,
    and IBM SDK 8.0.8.45 are already installed. This represents a typic
    traditional WAS ND environment.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image5.png)

    e.  **Close** and **Exit** Installation Manager.



2.  Start the WebSphere Integrated Solutions Console (WAS Admin
    Console).
    
    a.  Start the Deployment Manager with commands:
        
        cd /home/techzone/IBM/WebSphere/AppServer/profiles/Dmgr01/bin
        
        ./startManager.sh
        
    ![A screenshot of a computer screen AI-generated content may be
        incorrect.](./images/media/image6.png)
        
    The WAS Deployment Manager is started.
    
    b. Open a web browser window by clicking **Activities (A)** and
        clicking the **Firefox icon (B)**.

    ![A black and white text with a red circle and a red circle
 AI-generated content may be incorrect.](./images/media/image7.png)
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image8.png)

    c.  In the Firefox window, navigate to the WebSphere Integrated
     Solutions Console (WAS Admin Console) with URL:
     
        https://localhost:9043/ibm/console   (A)

    ![A close-up of a browser window AI-generated content may be
 incorrect.](./images/media/image9.png)
 
    > Note: If you see the message “Warning: Potential Security Risk
 Ahead..”, click **Advanced…** then click **Accept the Risk and
 Continue**.

    d. Accept the default user ID and Password and click **Log in (A)**.

    ![A screenshot of a login page AI-generated content may be
  incorrect.](./images/media/image10.png)

    > Note: the log in credentials are User ID: techzone Password: IBMDem0s\!

    e. In the WebSphere page click **Servers (A)** and then click **All
     servers** (B) to view the installed WebSphere servers.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image11.png)
 
    As you can see that there are two WAS 9.0.5.24 servers and one IHS
 9.0.5.24 server are listed.

    f. Stop the Deployment Manager with command:
    
        cd /home/techzone/IBM/WebSphere/AppServer/profiles/Dmgr01/bin

        ./stopManager.sh -username techzone -password IBMDem0s\!
 
    ![A screenshot of a computer error AI-generated content may be
 incorrect.](./images/media/image12.png)

3.  Install Liberty and MoRE through IBM Installation Manager.
    
    a. In the **terminal** window issue the following command to
        launch **IBM Installation Manager.**

        /home/techzone/IBM/InstallationManager_Group/eclipse/IBMIM

    b. In the IBM Installation Manager page, click **Install (A)**.

    ![A screenshot of a software package AI-generated content may be
 incorrect.](./images/media/image13.png)

    c. In the Install Packages page, check the following boxes: 
    
    - **IBM WebSphere
     Application Server Liberty Network Deployment box (A**)
     - **IBM Semeru Runtime Certified Edition Version 17 box (B)** 
     - **IBM Modernized Runtime Extension for Java (MoRE) box (C)**

     then click **Next (D)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image14.png)

    d.  Accept the license agreement by selecting its **option button
     (A)** and click **Next (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image15.png)

    e.  Accept the default installation directories and click Next (A).

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image16.png)

    f. Click **Next (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image17.png)

    g. Accept the Recommended Installation option and click **Next (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image18.png)

    h. Click **Next (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image19.png)

    i. Click **Install (A)**.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image20.png)
 
    This kicks off the installation process. After a few minutes the
 installation is completed.

    j. Click **Finish (A)** and **exit** the Installation Manager.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image21.png)

4.  Enable command assistance and log command assistance in WAS Admin
    Console.

    > **Note:** This configuration is needed to generate the wsadmin command assistance for UI driven tasks.

    a. Start the WebSphere Application Network Deployment (WAS ND)
     environment with commands:
    
        cd /home/techzone/IBM/WebSphere/AppServer/profiles/Dmgr01/bin
    
        ./startManager.sh

        cd /home/techzone/IBM/WebSphere/AppServer/bin
 
        ./startNode.sh -profileName AppSrv01
 
        ./startNode.sh -profileName AppSrv02

    b. Go to the WAS Admin Console page in web browser with URL:
    
        https://localhost:9043/ibm/console

    c.  In the console page, navigate to: **System administration (A)**
     **\>** **Console preferences (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image22.png)

    d. Select the following options and click **Apply (C)**:
    
    - **Enable command assistance notifications (A)**
    
    - **Log command assistance commands (B)**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./images/media/image23.png)

5.  View the WebSphere nodes where the managed Liberty Servers / Cluster
    will be deployed
    
    a. Navigate to **System administration (A)** \> **Nodes (B)**.

    ![A computer screen shot of a welcome page AI-generated content may be
 incorrect.](./images/media/image24.png)
 
    The Deployment Manager, Node01 and Node02 are all on the same VM in
 this lab environment and share the same application server binaries.
 
     In a distributed environment, you would need to install MoRE on each
 node where the MoRE capabilities are required, which includes also the
 Deployment Manager.
 
    Both nodes have **MoRE** installed, which is required to run `Managed Liberty Servers` on the nodes.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image25.png)

6.  Create a static WebSphere cluster of two managed Liberty Servers,
    one on each of the two nodes.
    
    a. Navigate to **Servers (A)** \> **Clusters (B)** \> **WebSphere
         application server clusters (C)**.

    ![A screenshot of a computer software AI-generated content may be
 incorrect.](./images/media/image26.png)

    b. Click **New (A)** to start the “create cluster” wizard.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image27.png)

    c. Step 1, set Cluster name to **MLSCluster (A)**. Leave the other
     fields as default and click **Next (B)**.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image28.png)

    d.  Step 2, configure the first cluster member with the following
     values and click **Next (D)**.

    - Member Name: **libertyServer1 (A)**
    - Select node: **rhel9-BaseNode01 (B)**
    - Select **Create the member using an application server template** and **default-managed-liberty-server** (**C)**

    ![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image29.png)

    e.  Step 3, configure the second cluster member with the following
     values and click **Add Member (C)**:

    - Member Name: **libertyServer2 (A)**
    - Select node: **rhel9-BaseNode02 (B)**

    ![A screenshot of a computer AI-generated content may be
incorrect.](./images/media/image30.png)

    f.  Check to make sure that the versions of **libertyServer1** and
     **libertyServer2** are **LIBERTY 25.0.0.6** and click **Next
     (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image31.png)

    g.  Step 4, Review the configuration summary and click **Finish (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image32.png)

7.  View the `command assistance` that displays the `wsadmin` commands that could be used to create the cluster.
    
    a.  Click the “**View administrative scripting commands for last
         action”** **link (A),** located under the **Command
         Assistance** section on the right side of the pane

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image33.png)
 
    A new window is displayed that shows the wsadmin commands that will
 run to create the cluster.
 
    The nice thing about this is that you can copy these commands and use
 them to build wsadmin scripts to automate the configuration for
 consistency and repeatability.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image34.png)

    b. **Close** the command assistance window


8.  **Save** the updates to the master configuration.
    
    a.  Click **Save (A).**

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image35.png)

    b. Wait until you see the message saying, “The configuration
     synchronization complete for cell.”, then click **OK (A)**.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image36.png)
 
    Now the changes have been synchronized and the **MLSCluster** is
 created.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image37.png)

9.  View and confirm the Liberty servers are defined in the
    **MLSCluster.**
    
    a.  Navigate to **Servers (A)** \> **Server Types (B)** \>
        **WebSphere application servers (C)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image38.png)

    Note the **liberty servers** that are defined in the cluster named
**MLSCluster**

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image39.png)

10. Install the **WhereAmI** Java 17 application into the
    **MLSCluster.**
    
    a. Navigate to **Applications (A)** \> **New Application (B)**.

    ![A screenshot of a software application AI-generated content may be
  incorrect.](./images/media/image40.png)

    b.  Click **New Enterprise Application (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image41.png)

    c. Under **Path to new application**, select **Local file system
     (A)** and click **Browse… (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image42.png)

    d.  If you have completed the IBM Application Modernization
     Accelerator (AMA) lab in this lab environment, you can navigate to
     **/home/techzone/demos/AMA/WhereAmI-2.0.0-Project/WhereAmI/target**
     directory, select **WhereAmI-2.0.01.war file (A)** and click
     **Open (B)**.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image43.png)
 
    Otherwise, you can navigate to
 **/home/techzone/demos/ManagedLiberty-MoRE** directory, select
 **WhereAmI-2.0.01.war file (A)** and click **Open (B)**.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image44.png)

    e. Set Target Runtime to **WebSphere Liberty (A)** and click **Next** (B).

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image45.png)

    f. Select **Fast Path (A)** and click **Next (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image46.png)

    g. Leave **Step 1** unchanged and click **Next (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image47.png)

    h.   On **Step 2**, map the application module by checking the **box
     next to WhereAmI-2.0.1.war (A),** selecting both **MLSCluster**
     and **webserver1 (B)** under **Cluster and servers**, and clicking
     **Apply** (C)

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image48.png)

    i. The **MLSCluster** and **webserver1** servers are added to the
     Module Server list, click **Next (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image49.png)

    j. On **Step 3**, click **Next (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image50.png)

    k. On Step 4, Set **Context Root** as **/Liberty (A)** and click
     **Next (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image51.png)

 
    l. View the `command assistance` that displays the `wsadmin` commands
     that could be used to install the application by clicking the **View administrative scripting commands for last action link (A)**, located under the **Command Assistance** section on the right side of the pane.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image52.png)
 
    The wsadmin command to install the WhereAmI application is displayed.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image53.png)

    m. Click **Finish (A)** on the Installation summary page, which will
     install the application.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image54.png)

    n. After the installation is completed, click **Save** **(A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image55.png)

    o. Wait for the message box to display the messages stating the
     configuration has been successfully synchronized, then click **OK
     (A)**.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image56.png)
 
    The application is now installed into the managed Liberty Servers in
 the **MLSCluster**.

11. View the application and see that it is installed on the
    **MLSCluster** of Liberty servers.
    
    a. Navigate to **Applications (A)** \> **Application Types (B)**
         \> **WebSphere enterprise applications** **(C)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image57.png)

    b. Click the **WhereAmI-2.0.1\_war (A)** that is shown in the list of applications

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image58.png)

    c. Click **Manage Modules (A)** under the **Modules** section on the
     **General Properties** page.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image59.png)

    d.  You can see that the application is mapped to the **MLSCluster**
     and the IBM HTTP Server **webserver1** as shown below. 
     
    Click **Cancel (A)**, do not make any changes to the configuration.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image60.png)

12. Start the servers in the **MLSCluster**.
    
    a.  Go to the WebSphere Integration Solutions Console and navigate
         to **Servers (A)** \> **Clusters (B)** \> **WebSphere
         application server clusters (C).**

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image61.png)

    b.  Select the checkbox next to **MLSCluster** (**A)** and click
     **Start (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image62.png)

13. Verify the Liberty servers are running.
    
    a.  Navigate to **Servers** \> **Server Types** \> **WebSphere
         application servers**, and you can see that the two Liberty
         servers are running.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image63.png)

14. After the **MLSCluster** is started, you can access the installed
    application through the Liberty server ports.
    
    a.  From the WAS Admin Console navigate to **Servers** \> **Server
         Types** \> **WebSphere application servers** and click
         **libertyServer1 (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image64.png)

    b.  Click **Ports (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image65.png)

    c.  Identify the HTTP port, which is **9084** in the screenshot below  (your port number might be different).

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image66.png)

    d. Open a new browser tab browser in the lab environment and navigate
     to the **WhereAmI** application using the context root
     **“/Liberty**”: 
     
        http://localhost:9084/Liberty/WhereAmI   (A)

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image67.png)
 
    The **WhereAmI** application Home page is displayed.

    e.  Repeat the same steps **`a`** through **`d`** but select
     **`libertyServer2`** and its HTTP port (the port is **9085** in the
     screenshot below and your port number might be different), you can
     also access the application running on **libertyServer2**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image68.png)

15. Review the Liberty server log file.
    
    a.  To review the Liberty server log file, open the File Manager
         by clicking **Activities (A)** and clicking the **File Manager
         icon (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image69.png)

    b. In the `File Manager`, navigate to
    `/home/techzone/IBM/WebSphere/AppServer/profiles/AppSrv01/managedLiberty/usr/servers/libertyServer1/logs` 
     directory and select the `messages.log` file (A).

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image70.png)
 
    Now you see the **libertyServer1** log file and you can see the
 Liberty version and Java version it is used and the application URL.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image71.png)

16. Generate and Propagate the Web Server plugin configuration to IBM
    HTTP Server.

    We have run the application in the individual Liberty server, now
 let’s config and run the application through the IBM HTTP server,
 IHS.

    a. From the terminal window, start the IBM HTTP Server with the
    command:

        cd /home/techzone/IBM/HTTPServer/bin
 
        ./apachectl start

    b.  Back to `WAS Admin Console` page and navigate to **Servers (A)** \>    **Server Types (B)** \> **Web Servers (C)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image72.png)

    c.  Check box next to **webserver1 (A),** click **Generate Plug-in (B)**
    to generate the web server plugin configuration file based on the
    current configuration of managed Liberty servers deployed in the
    **MLSCluster.**

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image73.png)
 
    The **Generate Plug-in** action generates an updated version of the
 “**plugin-cfg.xml**” file that the IBM HTTP Server uses to determine
 which WAS/Liberty servers the incoming request route from the HTTP
 server.

    d.  Check box next to **webserver1 (A),** click **Propagate Plug-in
    (B).**

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image74.png)
 
    The **Propagate Plug-in** action moves the updated version of the
 “**plugin-cfg.xml**” file to the defined location that the
 webserver1 is expecting to find the file based on the IBM HTTP Server
 configuration.
 
    The HTTP Server loads this configuration file into memory for routing  and load distribution of incoming requests.

17. View the IBM HTTP Server **plugin-cfg.xml** file to see how it is
    configured to route requests to the managed Liberty servers.
    
    a.  From the terminal window, run the command:

        gedit /home/techzone/IBM/WebSphere/Plugins/config/webserver1/plugin-cfg.xml &
 
    As you can see that the **`ServerCluster`** section defines the
 WebSphere cluster and managed Liberty servers and their ports that are
 configured for the HTTP server to route and distribute WebSphere
 requests.
 
    ![A screenshot of a computer screen AI-generated content may be
 incorrect.](./images/media/image75.png)
 
    The **UriGroup** maps the URL requests for “/Liberty” to the
 MLSCluster of managed Liberty servers.
 
    ![A screen shot of a computer code AI-generated content may be
 incorrect.](./images/media/image76.png)

    b. **Close** the **plugin-cfg.xml** file. DO NOT MAKE ANY UPDATES.


18. Access the IBM HTTP Server on port 8080

    a.  Open a new browser tab in **Firefox** browser in the lab environment
    and navigate to the HTTP server URL: 
    
        http://localhost:8080   (A)

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image77.png)

19. Run the **WhereAmI** application through the IBM HTTP Server that is
    running on port 8080

    a.  Open a new browser tab in the lab environment and navigate
    to the **WhereAmI** application using the context root
    **“/Liberty**”: 
    
        http://localhost:8080/Liberty/WhereAmI    (A)

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image78.png)
 
    The application page is displayed.

20. Change the Liberty server port to see what is going to happy to the
    IHS configuration.
    
    a.  From the WAS Console, navigate to **Servers** \> **Server
         Types** \> **WebSphere application servers** and select
         **libertyServer2 (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image79.png)

    b. Click **Ports (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image80.png)

    c.  Click the **default port link (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image81.png)

    d.  Change the default port value from **9085** to **9086 (A)** and click
     **Apply (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image82.png)

    e.  Click **Save (A)** to save the port number change.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image83.png)
 
    Notice that the warning message about `host aliases update`.

    f. Click **OK (A)** to confirm the change.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image84.png)
 
    The port now is changed to 9086.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image85.png)

    g.  To check if the port 9086 is in the host aliases or not, navigate
     to **Environment (A)** \> **Virtual Hosts (B)**.

    ![A screenshot of a software AI-generated content may be
  incorrect.](./images/media/image86.png)

    h.  Click **default\_host link (A)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image87.png)

    i.  Click **Host Aliases (A)**.

    ![A screenshot of a computer screen AI-generated content may be
 incorrect.](./images/media/image88.png)
 
    The Host Aliases is listed and as you can see that the port 9086 is
 not in this list and needs to be added.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image89.png)

    j. To add the new port number, click **New… (A)**.

    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image90.png)

    k. Enter **9086 (A)** and click **Apply (B)**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image91.png)

    l. Click **Save** and click **OK**. Now the port number **9086** is
     added to the Host Aliases.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image92.png)

    m. Next let’ check the IHS **`plugin-cfg.xml`** file to see if it is
     updated with the new port we just added. Form the terminal window,
     run the command:

        gedit /home/techzone/IBM/WebSphere/Plugins/config/webserver1/plugin-cfg.xml  &
 
    As you can see that the xml file has be automatically updated with the port number **9086**.
 
    ![A close-up of a computer screen AI-generated content may be
 incorrect.](./images/media/image93.png)

21. Test the high availability of the IHS plugin configuration.
    
    a.  From the WAS Console, navigate to **Servers** \> **Server
         Types** \> **WebSphere application servers** and check the box
         next to **libertyServer1 (A)** and click **Stop (B)** to stop
         **libertyServer1**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image94.png)

    b. Click **OK (A)**.

    ![A screenshot of a computer error AI-generated content may be
 incorrect.](./images/media/image95.png)
 
    The **libertyServer1** is now stopped.
 
    ![A screenshot of a computer AI-generated content may be
 incorrect.](./images/media/image96.png)

    c. Go back to the browser window and access **WhereAmI** application
     using the IHS URL: 
     
        http://localhost:8080/Liberty/WhereAmI 
        
    You can see that the application is still running on
     **libertyServer2** with the now port **9086**.

    ![A screenshot of a computer AI-generated content may be
  incorrect.](./images/media/image97.png)


## Summary

In this lab, you learned how Java 17 applications can be deployed and
managed via managed Liberty severs in an existing WebSphere Application
Server – ND cell using IBM Modernized Runtime Extension for Java (MoRE).

In this scenario, you explored:

  - Using the very familiar WebSphere Administrative console for
    administering Liberty servers

  - Create a WebSphere cluster of managed Liberty servers

  - Install Java 17 applications to the WebSphere cluster

  - View wsadmin commands generated by the command assistant in the
    admin console

  - View the managed Liberty server logs

  - Generate and propagate the WebSphere plugin configuration for IBM
    HTTP Server

  - Run the WhereAmI application through the HTTP server routing
    requests to the cluster of managed Liberty servers

![A blue and white logo Description automatically
generated](./images/media/image98.png)
