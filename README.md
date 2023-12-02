# PUSRDoc
USR-M300 Developer Document
## 1.Product Overview
### 1.1.Product Brief Introduction
 The M300 is a high-performance, scalable, and comprehensive edge gateway. The product integrates the functions of edge data collection, computing, active reporting, data reading and writing, linkage control, IO acquisition and control, etc., and the acquisition protocol includes standard Modbus protocol and a variety of common PLC protocols, as well as industry-specific protocols. The active reporting adopts the group reporting method and the customized Json reporting template to quickly realize the docking of server data formats. At the same time, the product also has routing and VPN and graphical programming functions, and graphical module design edge computing functions to meet customers' own design needs. The product supports TCP/MQTT(S) protocol communication and supports multi-channel connection; It supports Modbus RTU/TCP and OPC UA protocol conversion, and the product supports fast access to common platforms such as manned cloud, Alibaba Cloud, AWS, and Huawei Cloud.
 
  The product adopts Linux kernel, the main frequency is up to 1.2Ghz, the network adopts WAN/LAN plus 4G cellular design, the uplink transmission is more reliable, and the LAN port can be connected to external cameras and other devices, combined with its own routing function to achieve functional applications; The hardware integrates 2 channels of DI, 2 channels of DO, 2 channels of AI and 2 channels of RS485, which can not only realize the needs of industrial field control and collection, but also realize linkage control according to the data or status of various collection points. It can be widely used in a variety of industrial intelligent solutions such as smart farming and smart factory.
  
  The structure of the product adopts an expandable design, which can be combined and applied by expanding modules with different functions to better meet the needs of different scenarios for the number of IOs and communication interfaces. Convenient, fast and cost-saving.

### 1.2.Background on Node-RED's integration with the product
As the IoT and automation landscape continues to evolve, Node-RED is a key tool for connecting and managing a wide range of devices and services. Considering the characteristics of the "M300" product, we decided to integrate Node-RED into the "M300" to provide a more powerful and flexible solution. This integration will allow you to easily create customized, automated processes that connect various data sources and actions to each other for efficient, intelligent workflows.

## 2.Nord-Rader Foundation
### 2.1.Learn about Node-RED
Before using Node-RED in the M300 product, let's first understand some basic information about Node-RED. Node-RED is a programming tool originally developed for IBM and is currently part of the OpenJS Foundation. Node-RED is used to connect hardware devices, APIs, and online services together in new and interesting ways. It provides a browser-based editor that makes it easy to connect streams together using the various nodes in Node-RED and deploy them to their runtime environment with a single click.
### 2.2. Node-RED Editor Interface
图片  
The development interface of Node-RED is usually divided into the following main areas:

- `Toolbar`: The toolbar is located at the top of the interface and contains action buttons related to process editing and deployment. These buttons can be used to save, deploy, import/export processes, set up, and more.
- `Navigation panel`: The navigation panel is located on the left, right, and top sides and contains the following sections:
- `Process panel`: Displays the currently edited process, where you can create and switch between different processes.
- `Node Library`: Displays a list of available nodes, including built-in nodes and installed custom nodes.
- `Info Panel`: Provides information and help documentation about the selected node.
- `Editor area`: The editor area occupies most of the interface space and is used to create and edit processes. In this area, you can drag and drop nodes, - connect them, and configure node properties. The editor is a visual workspace.
- `Properties Panel`: The Properties panel is located on the side of the editor area and is used to configure the properties and settings of the selected node. When you select a node, the relevant properties and options will appear here so you can customize them.
- `Debug area`: The debug area is located at the bottom or side and is used to view the debug information of the process. This is where you can view the - --input and output data for the node to help diagnose the problem.

### 2.3. Basic Concepts and Terminology
When using Node-RED for process programming, it is important to have some basic concepts and terminology. Here are some basic concepts and terminology of Node-RED:

- 1.`Process`: A process is a unit of work made up of nodes that represents a set of related actions or tasks. In Node-RED, you can create multiple processes to organize and manage different functions. Each process has its own editing area.
- 2.`Nodes`: Nodes are the basic building blocks in Node-RED that perform a variety of operations and functions. A node can be an input node, a processing -node, or an output node. For example, an input node can get data from a sensor, a processing node can process the data, and an output node can send data to other devices or services.
- 3.`Traverse`: A traverse is a line between connecting nodes that represents the path of data flow. You can connect different nodes by dragging wires to define where the data flows.
- 4.`Editor`: A graphical user interface for Node-RED to create, edit, and deploy processes. The editor includes a visual workspace where you can drag and drop nodes, connect them, and configure node properties.
- 5.`Deploy`: Once you have created or edited the process, you can deploy it by clicking the "Deploy" button in the editor. Deploy will get the process running and save the changes to the Node-RED runtime.
- 6.`Node Library`: A node library is a list of the various available nodes. Node-RED includes built-in nodes and also supports the installation of custom nodes. You can drag and drop nodes from the node library into the editor to add new features.
- 7.`Message`: A message is a unit of data that is passed in Node-RED. It can be text, JSON objects, binary data, etc. The message is contained in the flow of wires and is processed and transformed by the nodes.
- 8.`Properties`: Nodes have configurable properties that define their behavior. You can customize the functions and settings of a node by editing its properties.
- 9.`Debugging`: Node-RED provides debugging tools to view the input and output data of nodes to help diagnose problems in the process.
- 10.`Process variables`: Process variables are data that is shared throughout the process. You can use process variables to pass and store information between different nodes.
- 11.`Global variables`: Global variables are data that is shared across all processes. They are accessible throughout the Node-RED instance.
- 12.`Node state`: A node can store data in its state to keep information between different message processes. This is useful for tracking the state of a node.  
These basic concepts and terminology can help you understand how Node-RED works and start creating your own automation processes. Node-RED's visual programming approach makes it ideal for IoT, automation, and data processing tasks.
## 3. Node introduction
### 3.1. Introduction to Generic Nodes/Streams
#### 3.1.1. Inject
Messages can be injected into the stream manually or periodically. The payload of a message can be of a variety of types, including strings, JavaScript objects, or current time.  
**Output**:

|Illustrate|Data Type|  
|----|----|
|payload|various|  
|The payload of the specified message.|  
|topic|string|  
|Optional properties that can be configured in the node.|  

**Detailed:**  
By using a specific payload, the injection node can start the flow. The default payload is the timestamp of the current time (in milliseconds, since January 1, 1970).  
The node also supports injecting strings, numbers, booleans, JavaScript objects, or stream/global context values.  
By default, nodes can be triggered manually by clicking the node button in the editor. It can also be set to be injected on a regular or scheduled basis.
Another optional setting is to inject once each time the flow is started.  
The maximum interval that can be specified is approximately 596 hours/24 days. However, if you are intervals of more than one day, it is recommended that you use a scheduler node to deal with power outages or reboots.  
Note: The options "Time Interval" and "Specific Time" use the standard cron system. This means that therefore "20 minutes" does not mean 20 minutes after that, but 20 minutes, 40 minutes per hour. If you want to set it to every 20 minutes from now on, then use the "Interval" option.  
Note: If you want to include line breaks in the string, you must use the Functions node to create the payload.  






