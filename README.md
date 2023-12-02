# PUSRDoc
USR-M300 Developer Document
## 1.Product Overview
### 1.1.Product Brief Introduction
- The M300 is a high-performance, scalable, and comprehensive edge gateway. The product integrates the functions of edge data collection, computing, active reporting, data reading and writing, linkage control, IO acquisition and control, etc., and the acquisition protocol includes standard Modbus protocol and a variety of common PLC protocols, as well as industry-specific protocols. The active reporting adopts the group reporting method and the customized Json reporting template to quickly realize the docking of server data formats. At the same time, the product also has routing and VPN and graphical programming functions, and graphical module design edge computing functions to meet customers' own design needs. The product supports TCP/MQTT(S) protocol communication and supports multi-channel connection; It supports Modbus RTU/TCP and OPC UA protocol conversion, and the product supports fast access to common platforms such as manned cloud, Alibaba Cloud, AWS, and Huawei Cloud.
- The product adopts Linux kernel, the main frequency is up to 1.2Ghz, the network adopts WAN/LAN plus 4G cellular design, the uplink transmission is more reliable, and the LAN port can be connected to external cameras and other devices, combined with its own routing function to achieve functional applications; The hardware integrates 2 channels of DI, 2 channels of DO, 2 channels of AI and 2 channels of RS485, which can not only realize the needs of industrial field control and collection, but also realize linkage control according to the data or status of various collection points. It can be widely used in a variety of industrial intelligent solutions such as smart farming and smart factory.
- The structure of the product adopts an expandable design, which can be combined and applied by expanding modules with different functions to better meet the needs of different scenarios for the number of IOs and communication interfaces. Convenient, fast and cost-saving.

### 1.2.Background on Node-RED's integration with the product
- As the IoT and automation landscape continues to evolve, Node-RED is a key tool for connecting and managing a wide range of devices and services. Considering the characteristics of the "M300" product, we decided to integrate Node-RED into the "M300" to provide a more powerful and flexible solution. This integration will allow you to easily create customized, automated processes that connect various data sources and actions to each other for efficient, intelligent workflows.

## 2.Nord-Rader Foundation
### 2.1.Learn about Node-RED
- Before using Node-RED in the M300 product, let's first understand some basic information about Node-RED. Node-RED is a programming tool originally developed for IBM and is currently part of the OpenJS Foundation. Node-RED is used to connect hardware devices, APIs, and online services together in new and interesting ways. It provides a browser-based editor that makes it easy to connect streams together using the various nodes in Node-RED and deploy them to their runtime environment with a single click.
### 2.2. Node-RED Editor Interface
- 图片
- The development interface of Node-RED is usually divided into the following main areas:

- `Toolbar`: The toolbar is located at the top of the interface and contains action buttons related to process editing and deployment. These buttons can be used to save, deploy, import/export processes, set up, and more.
- `Navigation panel`: The navigation panel is located on the left, right, and top sides and contains the following sections:
Process panel: Displays the currently edited process, where you can create and switch between different processes.
Node Library: Displays a list of available nodes, including built-in nodes and installed custom nodes.
Info Panel: Provides information and help documentation about the selected node.
Editor area: The editor area occupies most of the interface space and is used to create and edit processes. In this area, you can drag and drop nodes, connect them, and configure node properties. The editor is a visual workspace.
Properties Panel: The Properties panel is located on the side of the editor area and is used to configure the properties and settings of the selected node. When you select a node, the relevant properties and options will appear here so you can customize them.
Debug area: The debug area is located at the bottom or side and is used to view the debug information of the process. This is where you can view the input and output data for the node to help diagnose the problem.

