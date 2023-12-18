# PUSRDoc
USR-M300 Developer Document  
![](images/M300.png)  
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
#### **3.1.2. Debug**
Displays selected message properties in the debug sidebar tab and optionally the runtime log. By default it displays`msg.payload`, but can be configured to display any property, the full message or the result of a JSONata expression.  
**Details:**  
The debug sidebar provides a structured view of the messages it is sent, making it easier to understand their structure  
JavaScript objects and arrays can be collapsed and expanded as required. Buffer objects can be displayed as raw data or as a string if possible.  
Alongside each message, the debug sidebar includes information about the time the message was received, the node that sent it and the type of the message. Clicking on the source node id will reveal that node within the workspace.  
The button on the node can be used to enable or disable its output. It is recommended to disable or remove any Debug nodes that are not being used.  
The node can also be configured to send all messages to the runtime log, or to send short (32 characters) to the status text under the debug node. 
#### 3.1.3. Complete
Trigger a flow when another node completes its handling of a message. 
**Details:**  
If a node tells the runtime when it has finished handling a message, this node can be used to trigger a second flow.  
This node must be configured to handle the event for selected nodes in the flow. Unlike the Catch node, it does not provide a 'handle all' mode automatically applies to all nodes in the flow.  

Not all nodes will trigger this event - it will depend on whether they have been implemented to support this feature as introduced in Node-RED 1.0.  
#### 3.1.4. Catch
Catch errors thrown by nodes on the same tab. 
**Outputs:**  
| Description                                         | Type   |
| --------------------------------------------------- | ------ |
| error.message                                       | string |
| the error message.                                  |        |
| error.source.id                                     | string |
| the id of the node that threw the error.            |        |
| error.source.type                                   | string |
| the type of the node that threw the error.          |        |
| error.source.name                                   | string |
| the name, if set, of the node that threw the error. |        |  

**Details:**  
If a node throws an error whilst handling a message, the flow will typically halt. This node can be used to catch those errors and handle them with a dedicated flow.  
By default, the node will catch errors thrown by any node on the same tab. Alternatively it can be targetted at specific nodes, or configured to only catch errors that have not already been caught by a 'targeted' catch node.  
When an error is thrown, all matching catch nodes will receive the message.  
If an error is thrown within a subflow, the error will get handled by any catch nodes within the subflow. If none exists, the error will be propagated up to the tab the subflow instance is on.  
If the message already has a`error`property, it is copied to`_error`.
#### 3.1.5. status
Report status messages from other nodes on the same tab.  
**Outputs:**  
| Description  |  Type |
| --- | --- |
| status.text | string |
| the status text. |     |
| tstatus.source.type | string |
| the type of the node that reported status. |     |
| status.source.id | string |
| the id of the node that reported status. |     |
| status.source.name | string |
| the name, if set, of the node that reported status. |  

**Details:**  
This node does not produce a`payload`.  
By default the node reports status for all nodes on the same workspace tab. It can be configured to selectively report status for individual nodes.
#### 3.1.6. Link in
Create virtual wires between flows.  
**Details:**  
The node can be connected to any link out node that exists on any tab. Once connected, they behave as if they were wired together.  
The wires between link nodes are only displayed when a link node is selected. If there are any wires to other tabs, a virtual node is shown that can be clicked on to jump to the appropriate tab.  
**Note:** Links cannot be created going into, or out of, a subflow.
#### 3.1.6. Link call
Calls a flow that starts with a `link in` node and passes on the response.  
**Details:**  
This node can be connected to a `link in` node that exists on any tab. The flow connected to that node must end with a `link out` node configured in 'return' mode.  
When this node receives a message, it is passed to the connected `link in` node. It then waits for a response which it then sends on.  
If no response is received within the configured timeout, default 30 seconds, the node will log an error that can be caught using the `catch` node.
#### 3.1.6. Link out
Create virtual wires between flows.  
**Details:**  
This node can be configured to either send messages to all `link in` nodes it is connected to, or to send a response back to the `link call` node that triggered the flow.  
When in 'send to all' mode, the wires between link nodes are only displayed when the node is selected. If there are any wires to other tabs, a virtual node is shown that can be clicked on to jump to the appropriate tab.  
Note: Links cannot be created going into, or out of, a subflow.
#### 3.1.6. Comment
A node you can use to add comments to your flows.  
**Details:**  
The edit panel will accept Markdown syntax. The text will be rendered into this information side panel.  
### 3.2. Introduction to Function Nodes/Streams
#### 3.2.1. Function
A JavaScript function to run against the messages being received by the node.  
The messages are passed in as a JavaScript object called `msg`.  
By convention it will have a `msg.payload` property containing the body of the message.  
The function is expected to return a message object (or multiple message objects), but can choose to return nothing in order to halt a flow.  
The **On Start** tab contains code that will be run whenever the node is started. The **On Stop** tab contains code that will be run when the node is stopped.  
If the On Start code returns a Promise object, the node will not start handling messages until the promise is resolved.  
**Details:**  
See the [online documentation](http://nodered.org/docs/writing-functions.html) for more information on writing functions.  
**Sending messages**
The function can either return the messages it wants to pass on to the next nodes in the flow, or can call `node.send(messages)`.  
It can return/send:
- a single message object - passed to nodes connected to the first output  
- an array of message objects - passed to nodes connected to the corresponding outputs  
Note: The setup code is executed during the initialization of nodes. Therefore, if `node.send` is called in the setup tab, subsequent nodes may not be able to receive the message.  
If any element of the array is itself an array of messages, multiple messages are sent to the corresponding output.  
If null is returned, either by itself or as an element of the array, no message is passed on.  
**Logging and Error Handling**
To log any information, or report an error, the following functions are available:
```javascript
node.log("Log message")  
node.warn("Warning")  
node.error("Error")  
```
The Catch node can also be used to handle errors. To invoke a Catch node, pass `msg` as a second argument to `node.error`:  
```javascript
node.error("Error",msg);  
```
**Accessing Node Information**  
The following properties are available to access information about the node:  

- `node.id` - id of the node  
- `node.name` - name of the node  
- `node.outputCount` variables  
**Using environment variables**  
Environment variables can be accessed using `env.get("MY_ENV_VAR")`.  
#### 3.2.2. Switch
Route messages based on their property values or sequence position.  
**Details:**  
When a message arrives, the node will evaluate each of the defined rules and forward the message to the corresponding outputs of any matching rules.  
Optionally, the node can be set to stop evaluating rules once it finds one that matches.  
The rules can be evaluated against an individual message property, a flow or global context property, environment variable or the result of a JSONata expression.  
**Rules**  
There are four types of rule:  
1.**Value** rules are evaluated against the configured property.  
2. **Sequence** rules can be used on message sequences, such as those generated by the Split node.  
3. A JSONata **Expression** can be provided that will be evaluated against the whole message and will match if the expression returns a true value.  
**Notes**  
The is `true/false` and `is null` rules perform strict comparisons against those types. They do not convert between types.  
The `is empty` and `is not empty` rules can be used to test the length of Strings, Arrays and Buffers, or the number of properties an Object has. Neither rule will pass if the property being tested has a `boolean`, `null` or `undefined` value.  
**Handling message sequences**  
By default, the node does not modify the `msg.parts` property of messages that are part of a sequence.  
The **recreate message sequences** option can be enabled to generate new message sequences for each rule that matches. In this mode, the node will buffer the entire incoming sequence before sending the new sequences on. The runtime setting `nodeMessageBufferMaxLength` can be used to limit how many messages nodes will buffer.  
#### 3.2.3. Change
Set, change, delete or move properties of a message, flow context or global context.  
The node can specify multiple rules that will be applied in the order they are defined.  
**Details**
`Set`  
set a property. The value can be a variety of different types, or can be taken from an existing message or context property.  
`Change`  
search & replace parts of the property. If regular expressions are enabled, the "replace with" property can include capture groups, for example`$1`. Replace will only change the type if there is a complete match.  
`Delete`  
delete a property.  
`Move`
move or rename a property.  

The "expression" type uses the[JSONata](http://jsonata.org/)query and expression language.  

#### 3.2.4. Range
Maps a numeric value to a different range.  
**Inputs**  
`Inputs`  
The payload *must* be a number. Anything else will try to be parsed into a number and rejected if that fails.  
**Outputs**  
`Outputs`  
The value mapped to the new range.  
**Details**  
This node will linearly scale the received value. By default, the result is not constrained to the range defined in the node.  
*Scale and limit to target range*means that the result will never be outside the range specified within the target range.  
*Scale and wrap within the target range*means that the result will be wrapped within the target range.   
For example an input 0 - 10 mapped to 0 - 100.
| mode  | inout | output |
| ----- | ----- | ------ |
| scale | 12    | 120    |
| limit | 12    | 100    |
| wrap  | 12    | 20     |
#### 3.2.4.Template
Sets a property based on the provided template.  
**Input**  
| Description                                                   | Type   |
| ------------------------------------------------------------- | ------ |
| msg                                                           | object |
| A msg object containing information to populate the template. |        |
| template                                                      | string |
| A template to be populated from                               |        |
| msg.payload                                                   |        |
| If not configured in the edit panel, this can be set as a property of msg.            |        |  
  
**Output**  
| Description                                                   | Type   |
| ------------------------------------------------------------- | ------ |
| msg                                                           | object |
| a msg with a property set by populating the configured template with properties from the incoming msg. |        |  
  
**Details**  
By default this uses the[mustache](http://mustache.github.io/mustache.5.html)format, but this can be switched off if required.  

For example, when a template of:  
```javascript
Hello {{payload.name}}. Today is {{date}}
```
receives a message containing:  
```javascrip<font class="text-color-11" color="#e91e63"></font>t
{
  date: "Monday",
  payload: {
    name: "Fred"
  }
}
``` 
The resulting property will be:  
```javascript
Hello Fred. Today is Monday
```
It is possible to use a property from the flow context or global context. Just use`{{flow.name}}`or`{{global.name}}`, or for persistable store`store`use`{{flow[store].name}}`or`{{global[store].name}}`.  

**Note:**By default,*mustache*will escape any non-alphanumeric or HTML entities in the values it substitutes. To prevent this, use`{{{triple}}}`braces.  
#### 3.2.6. Delay
Delays each message passing through the node or limits the rate at which they can pass.  

** Inputs**
| Description                                                                                                                                                                                                                                                                                                                                                                                  | Type   |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| delay                                                                                                                                                                                                                                                                                                                                                                                        | Number |
| Sets the delay, in milliseconds, to be applied to the message. This option only applies if the node is configured to allow the message to override the configured default delay interval.                                                                                                                                                                                                    |        |
| rate                                                                                                                                                                                                                                                                                                                                                                                         | Number |
| Sets the rate value in milliseconds between messages. This node overwrites the existing rate value defined in the node configuration when it receives the message which contains <font class="text-color-2" color="#e91e63">msg.rate</font> value in milliSeconds. This option only applies if the node is configured to allow the message to override the configured default rate interval. |        |
| reset                                                                                                                                                                                                                                                                                                                                                                                        |        |
| If the received message has this property set to any value, all outstanding messages held by the node are cleared without being sent.                                                                                                                                                                                                                                                        |        |
| flush                                                                                                                                                                                                                                                                                                                                                                                        |        |
| If the received message has this property set to a numeric value then that many messages will be released immediately. If set to any other type (e.g. boolean), then all outstanding messages held by the node are sent immediately.                                                                                                                                                         |        |
| toFront              |        |
|When in rate limit mode, if the received message has this property set to boolean`true`, then the message is pushed to the front of the queue and will be released next. This can be used in combination with <font class="text-color-2" color="#e91e63">msg.flush=1</font> to resend immediately.               |        |

**Details**  
When configured to delay messages, the delay interval can be a fixed value, a random value within a range or dynamically set for each message. Each message is delayed independently of any other message, based on the time of its arrival.  
When configured to rate limit messages, their delivery is spread across the configured time period. The status shows the number of messages currently in the queue. It can optionally discard intermediate messages as they arrive.  
If set to allow override of the rate, the new rate will be applied immediately, and will remain in effect until changed again, the node is reset, or the flow is restarted.  
The rate limiting can be applied to all messages, or group them according to their `msg.topic` value. When grouping, intermediate messages are automatically dropped. At each time interval, the node can either release the most recent message for all topics, or release the most recent message for the next topic.   
Note: In rate limit mode the maximum queue depth can be set by a property in your *settings.js* file. 

#### 3.2.7. Trigger
When triggered, can send a message, and then optionally a second message, unless extended or reset.  
**Inputs**  
| delay  |  number |
| ------------ | ------------ |
| Sets the delay, in milliseconds, to be applied to the message. This option only applies if the node is configured to allow the message to override the configured default delay interval.  |   |
| reset  |   |
| If a message is received with this property, any timeout or repeat currently in progress will be cleared and no message triggered.  |           |

**Details**  
This node can be used to create a timeout within a flow. By default, when it receives a message, it sends on a message with a `payload` of `1`. It then waits 250ms before sending a second message with a `payload` of `0`. This could be used, for example, to blink an LED attached to a Raspberry Pi GPIO pin.  
The payloads of each message sent can be configured to a variety of values, including the option to not send anything. For example, setting the initial message to nothing and selecting the option to extend the timer with each received message, the node will act as a watchdog timer; only sending a message if nothing is received within the set interval.  
If set to a string type, the node supports the mustache template syntax.  
The delay between sending messages can be overridden by `msg.delay` if that option is enabled in the node. The value must be provided in milliseconds.  
If the node receives a message with a `reset` property, or a `payload` that matches that configured in the node, any timeout or repeat currently in progress will be cleared and no message triggered.  
The node can be configured to resend a message at a regular interval until it is reset by a received message.  
Optionally, the node can be configured to treat messages as if they are separate streams, using a msg property to identify each stream. Default `msg.topic`.  
The status indicates the node is currently active. If multiple streams are used the status indicates the number of streams being held.  
#### 3.2.8. Filter
Report by Exception (RBE) node - only passes on data if the payload has changed. It can also block unless, or ignore if the value changes by a specified amount (Dead- and Narrowband mode).  
**Inputs**
|Description   | Type  |
| ------------ | ------------ |
|payload   |number , string , (object)   |
|RBE mode will accept numbers, strings, and simple objects. Other modes must provide a parseable number.   |   |
|topic   |string   |
|if specified the function will work on a per topic basis. This property can be set by configuration.   |   |
|reset   |any   |
|if set clears the stored value for the specified msg.topic, or all topics if msg.topic is not specified.   |   |

**Outputs**
|Description   |Type   |
| ------------ | ------------ |
|payload  |as per input   |
|If triggered the output will be the same as the input.   |   |

**Details**  
In RBE mode this node will block until the `msg.payload`, (or selected property) value is different to the previous one. If required it can ignore the initial value, so as not to send anything at start.  
The [Deadband](https://en.wikipedia.org/wiki/Deadband) modes will block the incoming value unless its change is greater or greater-equal than ± the band gap away from a previous value.  
The Narrowband modes will block the incoming value, if its change is greater or greater-equal than ± the band gap away from the previous value. It is useful for ignoring outliers from a faulty sensor for example.  
Both in Deadband and Narrowband modes the incoming value must contain a parseable number and both also supports % - only sends if/unless the input differs by more than x% of the original value.  
Both Deadband and Narrowband allow comparison against either the previous valid output value, thus ignoring any values out of range, or the previous input value, which resets the set point, thus allowing gradual drift (deadband), or a step change (narrowband).  
Note: This works on a per `msg.topic` basis, though this can be changed to another property if desired. This means that a single filter node can handle multiple different topics at the same time.  
### 3.3. Network
#### 3.3.1. Mqtt in
Connects to a MQTT broker and subscribes to messages from the specified topic.  
**Outputs**  
|Description   |Type   |
| ------------ | ------------ |
| payload  |string , buffer   |
|a string unless detected as a binary buffer.   |   |
|topic   |string   |
|the MQTT topic, uses / as a hierarchy separator.   |   |
| qos  | number  |
|0, fire and forget - 1, at least once - 2, once and once only.   |   |
|retain   |boolean   |
|true indicates the message was retained and may be old.   |   |

**Detail**  
The subscription topic can include MQTT wildcards, + for one level, # for multiple levels.  
This node requires a connection to a MQTT broker to be configured. This is configured by clicking the pencil icon.  
Several MQTT nodes (in or out) can share the same broker connection if required.  

#### 3.3.2. Mqtt out
Connects to a MQTT broker and publishes messages.  
**Inputs**  
|Description   |Type   |
| ------------ | ------------ |
| payload  |string , buffer   |
|the payload to publish. If this property is not set, no message will be sent. To send a blank message, set this property to an empty String.   |   |
|topic   |string   |
|the MQTT topic to publish to.  |   |
| qos  | number  |
|0, fire and forget - 1, at least once - 2, once and once only. Default 0. |   |
|retain   |boolean   |
|set to true to retain the message on the broker. Default false.  |   |

**Details**  
`msg.payload` is used as the payload of the published message. If it contains an Object it will be converted to a JSON string before being sent. If it contains a binary Buffer the message will be published as-is.  
The topic used can be configured in the node or, if left blank, can be set by msg.topic.  
Likewise the QoS and retain values can be configured in the node or, if left blank, set by msg.qos and msg.retain respectively. To clear a previously retained topic from the broker, send a blank message to that topic with the retain flag set.  
This node requires a connection to a MQTT broker to be configured. This is configured by clicking the pencil icon.  
Several MQTT nodes (in or out) can share the same broker connection if required.  

#### 3.3.3 http in
Creates an HTTP end-point for creating web services.  
**Outputs**  
|Description   |Type   |
| ------------ | ------------ |
|payload   |   |
|For a GET request, contains an object of any query string parameters. Otherwise, contains the body of the HTTP request.   |   |
|req   |object  |
|An HTTP request object. This object contains multiple properties that provide information about the request.   |   |
|body - the body of the incoming request. The format will depend on the request.   |   |
|headers - an object containing the HTTP request headers.   |   |
|query - an object containing any query string parameters.   |   |
|params - an object containing any route parameters.   |   |
|cookies - an object containing the cookies for the request.   |   |
|files - if enabled within the node, an object containing any files uploaded as part of a POST request.   |   |
|res   |object   |
|An HTTP response object. This property should not be used directly; the HTTP Response node documents how to respond to a request. This property must remain attached to the message passed to the response node.   |   |

**Details**  
The node will listen on the configured path for requests of a particular type. The path can be fully specified, such as `/user`, or include named parameters that accept any value, such as `/user/:name`. When named parameters are used, their actual value in a request can be accessed under `msg.req.params`.  
For requests that include a body, such as a POST or PUT, the contents of the request is made available as `msg.payload`.  
If the content type of the request can be determined, the body will be parsed to any appropriate type. For example, `application/json` will be parsed to its JavaScript object representation.  
Note: this node does not send any response to the request. The flow must include an HTTP Response node to complete the request.  
#### 3.3.4 http response
Sends responses back to requests received from an HTTP Input node.  
**Inputs**  
|Description   |Type   |
| ------------ | ------------ |
|payload   | string  |
|The body of the response.   |   |
|statusCode   |number   |
|If set, this is used as the response status code. Default: 200.   |   |
|headers   |object   |
|If set, provides HTTP headers to include in the response.   |   |
| cookies  |object   |
|If set, can be used to set or delete cookies.   |   |  

**Details**  
The `statusCode` and `headers` can also be set within the node itself. If a property is set within the node, it cannot be overridden by the corresponding message property.  
Cookie handling  
The `cookies` property must be an object of name/value pairs. The value can be either a string to set the value of the cookie with default options, or it can be an object of options.  
The following example sets two cookies - one called name with a value of nick, the other called session with a value of 1234 and an expiry set to 15 minutes.  
```
msg.cookies = {
        name: 'nick',
        session: {
            value: '1234',
            maxAge: 900000
        }
    }
```
The valid options include:  

`domain` - (String) domain name for the cookie  
`expires` - (Date) expiry date in GMT. If not specified or set to 0, creates a session cookie  
`maxAge` - (String) expiry date as relative to the current time in milliseconds  
`path` - (String) path for the cookie. Defaults to /  
`value` - (String) the value to use for the cookie  
To delete a cookie, set its `value` to `null`.  

#### 3.3.5 http request
Sends HTTP requests and returns the response.  
**Inputs**  
|Description   |Type   |
| ------------ | ------------ |
|url   |string   |
|If not configured in the node, this optional property sets the url of the request.   |   |
|method   |string   |
|If not configured in the node, this optional property sets the HTTP method of the request. Must be one of `GET`, `PUT`, `POST`, `PATCH` or `DELETE`.   |   |
|headers   |Object   |
|Sets the HTTP headers of the request.   |   |
|cookies   |Object    |
|If set, can be used to send cookies with the request.   |   |
|payload   |   |
|Sent as the body of the request.   |   |
|rejectUnauthorized   |   |
|If set to `false`, allows requests to be made to https sites that use self signed certificates.   |   |
|followRedirects   |   |
|If set to `false` prevent following Redirect (HTTP 301).`true` by default   |   |
|requestTimeout   |   |
|If set to a positive number of milliseconds, will override the globally set `httpRequestTimeout` parameter.   |   |

**Outputs**  
|Description   |Type   |
| ------------ | ------------ |
|payload   |string , object , buffer   |
|The body of the response. The node can be configured to return the body as a string, attempt to parse it as a JSON string or leave it as a binary buffer.   |   |
|statusCode   | number   |
|The status code of the response, or the error code if the request could not be completed.   |   |
|headers   |Object  |
|An object containing the response headers.   |   |
|responseUrl   |Object   |
| If the response includes cookies, this property is an object of name/value pairs for each cookie.  |   |
|redirectList   |array   |
|If the request was redirected one or more times, the accumulated information will be added to this property. `location` is the next redirect destination. `cookies` is the cookies returned from the redirect source.   |   |

**Details**  
When configured within the node, the URL property can contain [mustache-style](https://mustache.github.io/mustache.5.html) tags. These allow the url to be constructed using values of the incoming message. For example, if the url is set to `example.com/{{{topic}}},` it will have the value of `msg.topic` automatically inserted. Using {{{...}}} prevents mustache from escaping characters like / & etc.  
The node can optionally automatically encode `msg.payload` as query string parameters for a GET request, in which case `msg.payload` has to be an object.  
Note: If running behind a proxy, the standard `http_proxy=...` environment variable should be set and Node-RED restarted, or use Proxy Configuration. If Proxy Configuration was set, the configuration take precedence over environment variable.  
Using multiple HTTP Request nodes  
In order to use more than one of these nodes in the same flow, care must be taken with the `msg.headers` property. The first node will set this property with the response headers. The next node will then use those headers for its request - this is not usually the right thing to do. If `msg.headers` property is left unchanged between nodes, it will be ignored by the second node. To set custom headers, `msg.headers` should first be deleted or reset to an empty object: `{}`.  
Cookie handling  
The `cookies` property passed to the node must be an object of name/value pairs. The value can be either a string to set the value of the cookie or it can be an object with a single `value` property.  
Any `cookies` returned by the request are passed back under the `responseCookies` property.  
Content type handling  
If `msg.payload` is an Object, the node will automatically set the content type of the request to `application/json` and encode the body as such.  

To encode the request as form data, `msg.headers["content-type"]` should be set to `application/x-www-form-urlencoded`.  

File Upload  
To perform a file upload, `msg.headers["content-type"]` should be set to `multipart/form-data` and the `msg.payload` passed to the node must be an object with the following structure:  
```
{
    "KEY": {
        "value": FILE_CONTENTS,
        "options": {
            "filename": "FILENAME"
        }
    }
}
```
The values of KEY, FILE_CONTENTS and FILENAME should be set to the appropriate. values.


#### 3.3.6. websocket in
WebSocket input node.  
By default, the data received from the WebSocket will be in `msg.payload`. The socket can be configured to expect a properly formed JSON string, in which case it will parse the JSON and send on the resulting object as the entire message.  
#### 3.3.7. websocket out
WebSocket out node.  
By default, `msg.payload` will be sent over the WebSocket. The socket can be configured to encode the entire `msg` object as a JSON string and send that over the WebSocket.  
If the message arriving at this node started at a WebSocket In node, the message will be sent back to the client that triggered the flow. Otherwise, the message will be broadcast to all connected clients.  
If you want to broadcast a message that started at a WebSocket In node, you should delete the `msg._session` property within the flow.  
#### 3.3.8. tcp in
Provides a choice of TCP inputs. Can either connect to a remote TCP port, or accept incoming connections.  
Note: On some systems you may need root or administrator access to access ports below 1024.  
#### 3.3.9. tcp out
Provides a choice of TCP outputs. Can either connect to a remote TCP port, accept incoming connections, or reply to messages received from a TCP In node.  
Only the `msg.payload` is sent.  
If `msg.payload` is a string containing a Base64 encoding of binary data, the Base64 decoding option will cause it to be converted back to binary before being sent.  
If `msg._session` is not present the payload is sent to all connected clients.  
Note: On some systems you may need root or administrator access to access ports below 1024.  
#### 3.3.10. tcp request
A simple TCP request node - sends the `msg.payload` to a server tcp port and expects a response.  
Connects, sends the "request", and reads the "response". It can either count a number of returned characters into a fixed buffer, match a specified character before returning, wait a fixed timeout from first reply and then return, sit and wait for data, or send then close the connection immediately, without waiting for a reply.  
The response will be output in `msg.payload` as a buffer, so you may want to .toString() it.  
If you leave tcp host or port blank they must be set by using the `msg.host` and `msg.port` properties in every message sent to the node.  
#### 3.3.11 udp in
A UDP input node, that produces a `msg.payload` containing a Buffer, string, or base64 encoded string. Supports multicast.  
It also provides `msg.ip` and `msg.port` set to the ip address and port from which the message was received.  
Note: On some systems you may need root or administrator access to use ports below 1024 and/or broadcast.  
#### 3.3.12 udp out
This node sends `msg.payload` to the designated UDP host and port. Supports multicast.  
You may also use `msg.ip` and `msg.port` to set the destination values, but the statically configured values have precedence.  
If you select broadcast either set the address to the local broadcast ip address, or maybe try 255.255.255.255, which is the global broadcast address.  
Note: On some systems you may need to be root to use ports below 1024 and/or broadcast.  
### 3.4. Sequence
#### 3.4.1. Split
Splits a message into a sequence of messages.  
**Inputs**  
| Description                                                                                                                      | Type                            |
| -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| Payload                                                                                                                          | object , string , array , buffe |
| The behaviour of the node is determined by the type of`msg.payload`:                                                             |                                 |
| **string**/**buffer**- the message is split using the specified character (default:`\n`), buffer sequence or into fixed lengths. |                                 |
| **array**- the message is split into either individual array elements, or arrays of a fixed-length.                              |                                 |
| **object**- a message is sent for each key/value pair of the object.      | 

**Outputs**  
| Description  | Type |
| --- | --- |
| Parts | object |
| This property contains information about how the message was split from the original message. If passed to the**join**node, the sequence can be reassembled into a single message. The property has the following properties: |     |
| **id**- an identifier for the group of messages |     |
| **index**- the position within the group |     |
| **count**- if known, the total number of messages in the group. See 'streaming mode' below. |     |
| **type**- the type of message - string/array/object/buffer |     |
| **ch**- for a string or buffer, the data used to the split the message as either the string or an array of bytes |     |
| **key**- or an object, the key of the property this message was created from. The node can be configured to also copy this value to another message properties, such as`msg.topic`. |     |
| **len**- the length of each message when split using a fixed length value |

**Details**  
This node makes it easy to create a flow that performs common actions across a sequence of messages before, using the**join**node, recombining the sequence into a single message.  
It uses the`msg.parts`property to track the individual parts of a sequence.  
Streaming mode  
The node can also be used to reflow a stream of messages. For example, a serial device that sends newline-terminated commands may deliver a single message with a partial command at its end. In 'streaming mode', this node will split a message and send each complete segment. If there is a partial segment at the end, the node will hold on to it and prepend it to the next message that is received.  
When operating in this mode, the node will not set the`msg.parts.count`property as it does not know how many messages to expect in the stream. This means it cannot be used with the**join**node in its automatic mode.  
#### 3.4.2. Join
Joins sequences of messages into a single message.  
There are three modes available:  
**automatic**  --When paired with the**split**node, it will automatically join the messages to reverse the split that was performed.  
**manual**--Join sequences of messages in a variety of ways.  
**reduce sequence**--Apply an expression against all messages in a sequence to reduce it to a single message.  

**Inputs:**  
| Description  | Type |
| --- | --- |
| Parts | object |
| To automatically join a sequence of messages, they should all have this property set. The**split**node generates this property but it can be manually created. It has the following properties: |     |
| **id**- an identifier for the group of messages |     |
| **index**- the position within the group |     |
| **count**- the total number of messages in the group |     |
| **type**- the type of message - string/array/object/buffer|     |
| **ch**- for a string or buffer, the data used to the split the message as either the string or an array of bytes |     |
| **key**- or an object, the key of the property this message was created from |     |
| **len**- the length of each message when split using a fixed length value |     |
| **Complete**：If set, the node will append the payload, and then send the output message in its current state. If you don't wish to append the payload, delete it from the msg. |

**Details**  
Automatic mode--Automatic mode uses the`parts`property of incoming messages to determine how the sequence should be joined. This allows it to automatically reverse the action of a**split**node.  
Manual mode--When configured to join in manual mode, the node is able to join sequences of messages into a number of different results:  
-a **string**or **buffer**-  created by joining the selected property of each message with the specified join characters or buffer.  
-an **array**- created by adding each selected property, or entire message, to the output array.  
-a **key/value object**- created by using a property of each message to determine the key under which the required value is stored.  
-a **merged object**- created by merging the property of each message under a single object.  

The other properties of the output message are taken from the last message received before the result is sent.  
A *count* can be set for how many messages should be received before generating the output message. For object outputs, once this count has been reached, the node can be configured to send a message for each subsequent message received.  
A *timeout* can be set to trigger sending the new message using whatever has been received so far. This timeout can be restarted by sending a message with the`msg.restartTimeout`property set.  
If a message is received with the`msg.complete`property set, the output message is finalised and sent. This resets any part counts.  
If a message is received with the`msg.reset`property set, the partly complete message is deleted and not sent. This resets any part counts.  

Reduce Sequence mode
When configured to join in reduce mode, an expression is applied to each message in a sequence and the result accumulated to produce a single message.  

`Initial value`-The initial value of the accumulated value (`$A`).  
`Reduce expression`-A JSONata expression that is called for each message in the sequence. The result is passed to the next call of the expression as the accumulated value. In the expression, the following special variables can be used:  
- `$A`: the accumulated value,  
- `$I`: index of the message in the sequence,  
- `$N`: number of messages in the sequence.  

`Fix-up expression`-An optional JSONata expression that is applied after the reduce expression has been applied to all messages in the sequence. In the expression, following special variables can be used:  
- `$A`: the accumulated value,  
- `$N`: number of messages in the sequence.  

By default, the reduce expression is applied in order, from the first to the last message of the sequence. It can optionally be applied in reverse order.  

**Example:** the following settings, given a sequence of numeric values, calculates the average value:  
- **Reduce expression**:`$A+payload`  
- **Initial value**:`0`  
- **Fix-up expression**:`$A/$N`  

Storing messages-This node will buffer messages internally in order to work across sequences. The runtime setting`nodeMessageBufferMaxLength`can be used to limit how many messages nodes will buffer.  

#### 3.4.3. Sort
A function that sorts message property or a sequence of messages.  
When configured to sort message property, the node sorts array data pointed to by specified message property.  
When configured to sort a sequence of messages, it will reorder the messages.  
The sorting order can be:  
- **ascending**,
- **descending**.
For numbers, numerical ordering can be specified by a checkbox.  
Sort key can be element value or JSONata expression for sorting property value, or message property or JSONata expression for sorting a message sequence.  
When sorting a message sequence, the sort node relies on the received messages to have`msg.parts`set. The split node generates this property, but can be manually created. It has the following properties:  
- `id`- an identifier for the group of messages  
- `index`- the position within the group  
- `count`- the total number of messages in the group  
**Note:** This node internally keeps messages for its operation. In order to prevent unexpected memory usage, maximum number of messages kept can be specified. Default is no limit on number of messages.  
- `nodeMessageBufferMaxLength`property set in**settings.js**.  

#### 3.4.4. Batch
Creates sequences of messages based on various rules.  
**Details**  
**Number of messages**-groups messages into sequences of a given length. The**overlap**option specifies how many messages at the end of one sequence should be repeated at the start of the next sequence.  
**Time interval**-groups messages that arrive within the specified interval. If no messages arrive within the interval, the node can optionally send on an empty message.  
**Concatenate Sequences**-creates a message sequence by concatenating incoming sequences. Each message must have a`msg.topic`property and a`msg.parts`property identifying its sequence. The node is configured with a list of`topic`values to identify the order sequences are concatenated.  

Storing messages  
This node will buffer messages internally in order to work across sequences. The runtime setting`nodeMessageBufferMaxLength`can be used to limit how many messages nodes will buffer.  

If a message is received with the`msg.reset`property set, the buffered messages are deleted and not sent.  

### 3.5. Parser
#### 3.5.1. csv
Converts between a CSV formatted string and its JavaScript object representation, in either direction.  
**Inputs:**
| Description  | Type |
| --- | --- |
| payload | object , array , string |
| A JavaScript object, array or CSV string. |

**Outputs:**
| Description                                                                                                                                                                                                                          | Type                    |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------- |
| payload                                                                                                                                                                                                                              | object , array , string |
| - If the input is a string it tries to parse it as CSV and creates a JavaScript object of key/value pairs for each line. The node will then either send a message for each line, or a single message containing an array of objects. |                         |
| - If the input is a JavaScript object it tries to build a CSV string.                                                                                                                                                                |                         |
| - If the input is an array of simple values, it builds a single line CSV string.                                                                                                                                                     |                         |
| - If the input is an array of arrays, or an array of objects, a multiple-line CSV string is created.                                                                                                                                                                                                                                     |                         |

**Details**  
The column template can contain an ordered list of column names. When converting CSV to an object, the column names will be used as the property names. Alternatively, the column names can be taken from the first row of the CSV.  
When converting to CSV, the columns template is used to identify which properties to extract from the object and in what order.   
If the input is an array then the columns template is only used to optionally generate a row of column titles.  
The node can accept a multi-part input as long as the`parts`property is set correctly, for example from a file-in node or split node.  
If outputting multiple messages they will have their`parts`property set and form a complete message sequence.  
**Note:**the column template must be comma separated - even if a different separator is chosen for the data.  

#### 3.5.2. html
Extracts elements from an html document held in`msg.payload`using a CSS selector.  
**Inputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| the html string from which to extract elements.|     |
| select | string |
| if not configured in the edit panel the selector can be set as a property of msg. |
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | array , string |
| the result can be either a single message with a payload containing an array of the matched elements, or multiple messages that each contain a matched element. If multiple messages are sent they will also have`parts`set.|     |

**Details**  
This node supports a combination of CSS and jQuery selectors. See the[css-select documentation](https://github.com/fb55/CSSselect#user-content-supported-selectors)for more information on the supported syntax.  

#### 3.5.3. json
Converts between a JSON string and its JavaScript object representation, in either direction.  
**Inputs:**  
| Description  | Type |
| --- | --- |
| payload |  object , string |
| A JavaScript object or JSON string.|     |
| schema | object , string |
| An optional JSON Schema object to validate the payload against. The property will be deleted before the`msg`is sent to the next node. |

**Outputs:**  
| Description                                                                                                  | Type            |
| ------------------------------------------------------------------------------------------------------------ | --------------- |
| payload                                                                                                      | object , string |
| - If the input is a JSON string it tries to parse it to a JavaScript object.                                 |                 |
| - If the input is a JavaScript object it creates a JSON string. The string can optionally be well-formatted. |                 |
| schemaError                                                                                                  | array           |
| If JSON schema validation fails, the catch node will have a`schemaError`property containing an array of errors.                                                                                                             |                 |

**Details**  
By default, the node operates on`msg.payload`, but can be configured to convert any message property.  
The node can also be configured to ensure a particular encoding instead of toggling between the two. This can be used, for example, with the`HTTP In`node to ensure the payload is a parsed object even if an incoming request did not set its content-type correctly for the HTTP In node to do the conversion.  
If the node is configured to ensure the property is encoded as a String and it receives a String, no further checks will be made of the property. It will not check the String is valid JSON nor will it reformat it if the format option is selected.  
For more details about JSON Schema you can consult the specification[here](http://json-schema.org/latest/json-schema-validation.html).  

#### 3.5.4. xml

Converts between an XML string and its JavaScript object representation, in either direction.  
**Inputs:**
| Description  | Type |
| --- | --- |
| payload |  object , string |
| A JavaScript object or XML string. |     |
| options | object |
| This optional property can be used to pass in any of the options supported by the underlying library used to convert to and from XML. See[the xml2js docs](https://github.com/Leonidas-from-XIV/node-xml2js/blob/master/README.md#options)for more information. |

**Outputs:**
| Description                                                                             | Type            |
| --------------------------------------------------------------------------------------- | --------------- |
| payload                                                                                 | object , string |
| - If the input is a string it tries to parse it as XML and creates a JavaScript object. |                 |
| - If the input is a JavaScript object it tries to build an XML string.                                     |                |

**Details**  
When converting between XML and an object, any XML attributes are added as a property named`$`by default. Any text content is added as a property named`_`. These property names can be specified in the node configuration.  

For example, the following XML will be converted as shown:
```javascript
<p class="tag">Hello World</p>
```
```javascript
{
  "p": {
    "$": {
      "class": "tag"
    },
    "_": "Hello World"
  }
}
```
#### 3.5.5. yaml
Converts between a YAML formatted string and its JavaScript object representation, in either direction.  
**Inputs:**
| Description  | Type |
| --- | --- |
| payload |  object , string |
| A JavaScript object or YAML string. |     |

**Outputs:**
| Description                                                                             | Type            |
| --------------------------------------------------------------------------------------- | --------------- |
| payload                                                                                 | object , string |
| - If the input is a YAML string it tries to parse it to a JavaScript object. |                 |
| - If the input is a JavaScript object it creates a YAML string.                                     |                |

### 3.6. Storage

#### 3.6.1. write File
Writes`msg.payload`to a file, either adding to the end or replacing the existing content. Alternatively, it can delete the file.  

**Inputs:**  
| Description                                                                                    | Type   |
| ---------------------------------------------------------------------------------------------- | ------ |
| filename                                                                                       | string |
| If not configured in the node, this optional property sets the name of the file to be updated. |        |
| encoding                                                                                               |  string      |
|  If encoding is configured to be set by msg, then this optional property can set the encoding.                                                                                              |        |
**Outputs:**  
Each message payload will be added to the end of the file, optionally appending a newline (\n) character between each one.  

**Details**  
Each message payload will be added to the end of the file, optionally appending a newline (\n) character between each one.  
If`msg.filename`is used the file will be closed after every write. For best performance use a fixed filename.  
It can be configured to overwrite the entire file rather than append. For example, when writing binary data to a file, such as an image, this option should be used and the option to append a newline should be disabled.  
Encoding of data written to a file can be specified from list of encodings.  
Alternatively, this node can be configured to delete the file.  

#### 3.6.2. read file
Reads the contents of a file as either a string or binary buffer.  

**Inputs:**  
| Description                                                                                    | Type   |
| ---------------------------------------------------------------------------------------------- | ------ |
| filename                                                                                       | string |
| if not set in the node configuration, this property sets the filename to read. |        |

**Outputs:**  

| Description                                                   | Type            |
| ------------------------------------------------------------- | --------------- |
| payload                                                       | string , buffer |
| The contents of the file as either a string or binary buffer. |                 |
| filename                                                              |   string              |
| If not configured in the node, this optional property sets the name of the file to be read.                                                              |                 |
**Details**  
The filename should be an absolute path, otherwise it will be relative to the working directory of the Node-RED process.  
On Windows, path separators may need to be escaped, for example:`\\Users\\myUser`.  
Optionally, a text file can be split into lines, outputting one message per line, or a binary file split into smaller buffer chunks - the chunk size being operating system dependant, but typically 64k (Linux/Mac) or 41k (Windows).  
When split into multiple messages, each message will have a`parts`property set, forming a complete message sequence.  
Encoding of input data can be specified from list of encodings if output format is string.  
Errors should be caught and handled using a Catch node.  

#### 3.6.3. Watch

Watches a directory or file for changes.  
You can enter a list of comma separated directories and/or files. You will need to put quotes "..." around any that have spaces in.  
The full filename of the file that actually changed is put into`msg.payload`and`msg.filename`, while a stringified version of the watch list is returned in`msg.topic`.  
`msg.file`contains just the short filename of the file that changed.`msg.type`has the type of thing changed, usually*file*or*directory*, while`msg.size`holds the file size in bytes.  
Of course in Linux,*everything*is a file and thus can be watched...  
**Note:** The directory or file must exist in order to be watched. If the file or directory gets deleted it may no longer be monitored even if it gets re-created.  

### 3.7. dashboard

#### 3.7.1. Button
Adds a button to the user interface.  
Clicking the button generates a message with`msg.payload`set to the**Payload**field. If no payload is specified, the node id is used.  
The**Size**defaults to 3 by 1.  
The**Icon**can be defined, as either a [Material Design icon](https://klarsys.github.io/angular-material-icons/)*(e.g. 'check', 'close')*or a [Font Awesome icon](https://fontawesome.com/v4.7.0/icons/)*(e.g. 'fa-fire')*, or a [Weather icon](https://github.com/Paul-Reed/weather-icons-lite/blob/master/css_mappings.md). You can use the full set of google material icons if you add 'mi-' to the icon name. e.g. 'mi-videogame_asset'.  
The colours of the text and background may be set. They can also be set by a message property by setting the field to the name of the property, for example`{{background}}`. You don't need to prepend the*msg.*part of the property name.  
The label and icon can also be set by a message property by setting the field to the name of the property, for example `{{topic}}` or `{{myicon}}`.  
If set to pass through mode a message arriving on the input will act like pressing the button. The output payload will be as defined in the node configuration.  
The incoming**topic**field can be used to set the `msg.topic` property that is output.  
Setting `msg.enabled` to `false` will disable the button.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className`string property.  

#### 3.7.2. Dropdown
Adds a dropdown select box to the user interface.  
Multiple value / label pairs can be added as required. If the label is not specified the value will be used for both.  
The configured value of the selected item will be returned as`msg.payload`.  
Setting`msg.payload`to one of the item values will preset the choice in the dropdown. If using the multi-select option then the payload should be an array of values.  
Optionally the user search term can deleted if the`msg.resetSearch`property is present and set to *true*.  
Optionally the**Topic**field can be used to set the`msg.topic`property.  

The Options may be configured by inputting`msg.options`containing an array. If just text then the value will be the same as the label, otherwise you can specify both by using an object of`"label":"value"`pairs :  

`[ "Choice 1", "Choice 2", {"Choice 3":"3"} ]`  

If the "Allow multiple selections" output option is enabled - the result will be returned as an array instead of a string.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a`msg.className`string property.  

#### 3.7.3. Switch
Adds a switch to the user interface.  

Each change in the state of the switch will generate a`msg.payload`with the specified **On** and **Off** values.  
The **On/Off Color** and **On/Off Icon** are optional fields. If they are all present, the default toggle switch will be replaced with the relevant icons and their respective colors.  
The **On/Off Icon** field can be either a [Material Design icon](https://klarsys.github.io/angular-material-icons/) *(e.g. 'check', 'close')* or a [Font Awesome icon](https://fontawesome.com/v4.7.0/icons/) *(e.g. 'fa-fire')* , or a [Weather icon](https://github.com/Paul-Reed/weather-icons-lite/blob/master/css_mappings.md). You can use the full set of google material icons if you add 'mi-' to the icon name. e.g. 'mi-videogame_asset'.  
In pass through mode the switch state can be updated by an incoming`msg.payload`with the specified values, that must also match the specified type (number, string, etc). When not in passthrough mode then the icon can either track the state of the output - or the input msg.payload, in order to provide a closed loop feedback.  
The label can also be set by a message property by setting the field to the name of the property, for example`{{msg.topic}}`.  
If a**Topic**is specified, it will be added to the output as `msg.topic`.  
Setting`msg.enabled`to`false`will disable the switch widget.  
If a**Class**is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a`msg.className`string property.  

#### 3.7.4. Slider

Adds a slider widget to the user interface.  

The user can change its value between the limits (**min**and**max**). Each value change will generate a message with the value set as **payload** .  

A vertical slider can be created by setting the size so that the height is greater than the width.  

The slider can be reversed by setting the min value larger than the max value. e.g. min 100, max 0.  

If a**Topic**is specified, it will be added as `msg.topic` .  

An input `msg.payload` will be converted to a number, and used to preset a value. The**min**value will be used if conversion fails, and it will update the user interface. If the value changes, it will also be passed to the output.  

The label can also be set by a message property by setting the field to the name of the property, for example `{{msg.topic}}`.  

Setting `msg.enabled` to `false` will disable the slider output.  

If a**Class**is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className` string property.  

#### 3.7.5. Numeric

Adds a numeric input widget to the user interface.  
The user can set the value between the limits (**min**and**max**). Each value change will generate a`msg.payload`.  
If**Topic**is specified, it will be added as`msg.topic`.  
Any input`msg.payload`will be converted to a number, the**min**value will be used if conversion fails, and it will update the user interface. If the value changes, it will also be passed to the output.  
The **Value Format** field can be used to change the displayed format. For example, a **Value Format **of`{{value}} %`with a value of **23** will show **23 %** on the user interface. The**Value Format**field can contain HTML or Angular filters to format the output (eg:`&deg;`will show the degree symbol).  
Setting the Value Format field to`{{msg.payload}}`will make the input field editable so you can type in a number.  
The label can also be set by a message property by setting the field to the name of the property, for example`{{msg.topic}}`.  
Setting`msg.enabled`to`false`will disable the widget output.  
If a**Class**is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className` string property.  

#### 3.7.6. text input

Adds a text input field to the user interface. Mode can be regular text, email or color picker.  
Any input is sent as `msg.payload`. If set to ‘pass through mode’ an arriving `msg.payload`will be used if it is different from the existing text in the input field. This allows you to preset the text of the input field.  
The **Delay** *(default : 300ms)* sets the amount of time in milliseconds before the output is sent. Setting to `0` waits for "Enter" or "Tab" key to send. Enter will send payload but remain in focus. Tab will send payload and move to next field. Clicking elsewhere will also send the payload.  
The email mode will color in red if it is not a valid address and will return undefined.  
The time input type returns a number of milliseconds from midnight.  
Not all browsers support the *week* and*month*input types, and may return*undefined*. Please test your target browser(s) before use.  
If a **Topic** is specified, it will be added as `msg.topic`.  
Setting `msg.enabled` to `false` will disable the input.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className` string property.  

#### 3.7.7. date picker
Adds a date picker widget to the user interface.  
The date display can be formatted in the Dashboard - Site tab using [moment.js](https://momentjs.com/docs/#/displaying/) formatting. For example `MM/DD/YYYY` , `Do MMM YYYY`  or `YYYY-MM-DD`.  
Setting `msg.enabled` to `false`will disable the input.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className` string property.  

#### 3.7.8. colour picker
Adds a colour picker to the dashboard.  
If the group width is 4 or greater then the picker can be set to be visible at all times.  
**Format** can be rgb, hex, hex8, hsv, or hsl. Transparency is supported for all except hex.  
If a **Topic** is specified, it will be added as `msg.topic`.  
Setting `msg.enabled` to `false` will disable the input.  
If set to ‘pass through mode’ a message arriving on the input will be evaluated for any colour format available as Format. If the conversion fails #000000 will be used.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a  `msg.className`  string property.  

#### 3.7.9. Form

Adds a form to user interface.  
Helps to collect multiple value from the user on submit button click as an object in `msg.payload`   
Multiple input elements can be added using add elements button  
Each element contains following components:  
- **Label**: Value that will be the label of the element in the user interface  
- **Name**: Represents the key (variable name) in the`msg.payload`in which the value of the corresponding element present  
- **Type**: Drop down option to select the type of input element  
- **Required**: On switching on the user has to supply the value before submitting  
- **Rows**: number of UI rows for multiline text input  
- **Delete**: To remove the current element from the form  
Optionally the **Topic** field can be used to set the `msg.topic` property.  
The Cancel button can be hidden by setting it's value to be blank "".  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className` string property.  

#### 3.7.10. Text

Will display a non-editable text field on the user interface.  
Each received `msg.payload` will update the text based on the provided **Value Format** .
The **Value Format** field can be used to change the displayed format and can contain valid HTML and [Angular filters](https://scotch.io/tutorials/all-about-the-built-in-angularjs-filters).  
For example: `{{value | uppercase}} &deg;` will uppercase the payload text and add the degree symbol.  
The label can also be set by a message property by setting the field to the name of the property, for example `{{msg.topic}}`.  
The following icon fonts are also available: [Material Design icon](https://klarsys.github.io/angular-material-icons/) *(e.g. 'check', 'close')* or a[Font Awesome icon](https://fontawesome.com/v4.7.0/icons/)*(e.g. 'fa-fire')*, or a [Weather icon](https://github.com/Paul-Reed/weather-icons-lite/blob/master/css_mappings.md). You can use the full set of google material icons if you add 'mi-' to the icon name. e.g. 'mi-videogame_asset'.  
The widget also has a class of `nr-dashboard-widget-{the_widget_label_with_underscores}` which can be used for additional styling if required. You may need to use the*!important*flag to override the theme.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a `msg.className` string property.  

#### 3.7.11. Gauge

Adds a gauge type widget to the user interface.  
The `msg.payload` is searched for a numeric*value*and is formatted in accordance with the defined **Value Format** , which can then be formatted using [Angular filters](https://docs.angularjs.org/api/ng/filter).  
For example : `{{value | number:1}}%` will round the value to one decimal place and append a % sign.  
The colours of each of 3 sectors can be specified and the gauge will blend between them. The colours should be specified in hex (#rrggbb) format.  
If you specify numbers for the sectors then the colours changes per sector. If not specified the colours are blended across the total range.  
The gauge has several modes. Regular gauge, donut, compass and wave.  
The label can also be set by a message property by setting the field to the name of the property, for example `{{msg.topic}}`.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a`msg.className`string property.  


#### 3.7.12. Chart
Plots the input values on a chart. This can either be a time based line chart, a bar chart (vertical or horizontal), or a pie chart.  
Each input`msg.payload`value will be converted to a number. If the conversion fails, the message is ignored.  
Minimum and Maximum**Y**axis values are optional. The graph will auto-scale to any values received.  
Multiple series can be shown on the same chart by using a different`msg.topic`value on each input message. Multiple bars of the same series can be shown by using the`msg.label`property.  
The**X**axis defines a time window or a maximum number of points to display. Older data will be automatically removed from the graph. The axis labels can be formatted using a [Moment.js time formatted](https://momentjs.com/docs/#/displaying/format/) string.  
Inputting a`msg.payload`containing a blank array`[]`will clear the chart.  
See **[this information](https://github.com/node-red/node-red-dashboard/blob/master/Charts.md)** for how to pre-format data to be passed in as a complete chart.  
The **Blank label** field can be used to display some text before any valid data is received.  
The label can also be set by a message property by setting the field to the name of the property, for example `{{msg.topic}}`.  
The node output contains an array of the chart state that can be persisted if needed. This can be passed into the chart node to re-display the persisted data.  
If a**Class**is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a`msg.className`string property.  

#### 3.7.13. audio out

Plays audio or text to speech (TTS) in the dashboard.  

To work the dashboard web page must be open.  

Expects`msg.payload`to contain a buffer of a **wav** or **mp3** file.  

If your browser has native support for Text-to-Speech then a`msg.payload`can also be a**string**to be read aloud.  

Optionally setting`msg.level`from 0 to 100 will change the volume from 0 to 100%. Default is 100%. In audio mode you can go up to 300, but you may get distortion.  

When a`msg.reset`is available with value`true`, then playback of the current audio fragment will be stopped.  

The **node status** reflects the current playback status:  

- **started:** the audio fragment playback has been started.  
- **reset:** the audio fragment playback has been reset (i.e. stopped before completed).  

As soon as the audio fragment playback is completed, the node status will be cleared automatically.  
#### 3.7.14. Notification

Shows `msg.payload` as a popup notification or OK / Cancel dialog message on the user interface.  
If a `msg.topic` is available it will be used as the title.  
If you do not set an optional border highlight colour, then it can be set dynamically by`msg.highlight`.  
You may also configure the position and duration of the toast notifications. If you leave the timeout blank it can be set by `msg.timeout` . This does not apply to OK/Cancel dialogs.  
The dialog returns a`msg.payload`string of whatever you configure the button label(s) to be. The second (cancel) button is optional, as is the return value of`msg.topic`.  
If you select 'OK, Cancel and Input' mode then`msg.payload`will contain any text input by the user, rather than the OK button text.  
The OK and Cancel button labels can be replaced by using`msg.ok`and`msg.cancel`  
Sending a blank payload will remove any active dialog without sending any data.  
If a **Class** is specified, it will be added to the parent element. This way you can style the card and the elements inside it with custom CSS.  

#### 3.7.15. ui control

Allows dynamic control of the Dashboard.  

The default function is to change the currently displayed tab.`msg.payload`should either be an object of the form`{"tab":"my_tab_name"}`, or just be the**tab name**or**numeric index**(from 0) of the tab or link to be displayed.  

Sending a blank tab name "" will refresh the current page. You can also send "+1" for next tab and "-1" for previous tab.  

Dashboard pages (i.e. "tabs") can be controlled by sending a`msg.payload`object with the format.  
```javascript
{"tabs": {"hide": "tab_name_to_hide", "disable": ["secret_tab", "unused_stuff"]}}
```
There are 2 toggle states available: **show**/**hide** and **enable**/**disable**  

Visibility of individual groups of widgets can controlled by a payload like  
```javascript
{"group": {"hide": ["tab_name_group_name_with_underscores"], "show": ["reveal_another_group"], "focus": true}}
```
where **focus** is optional and will cause the screen to scroll to show that group if required. You can also use properties `open` and `close` to set the state of a group that can be controlled by the user. The group names are the IDs of the groups and are typically formed from the*tab name* plus *group name* joined with underscores replacing all spaces.  

When any browser client connects or loses connection, changes tab, or expands or collapses a group this node will emit a`msg`containing:  

- `payload`-*connect*,*lost*,*change*, or *group*.  
- `socketid`- the ID of the socket (this will change every time the browser reloads the page).  
- `socketip`- the ip address from where the connection originated.  
- `tab`- the number of the tab. (only for 'change' event).  
- `name`- the name of the tab. (only for 'change' event).  
- `group`- the name of the group. (only for 'group' event).  
- `open`- the state of the group. (only for 'group' event).  

Optional - report only connect events - useful to use to trigger a resend of data to a new client without needing to filter out other events.  

#### 3.7.16. Template
The template widget can contain any valid html and Angular/Angular-Material directives.  
This node can be used to create a dynamic user interface element that changes its appearance based on the input message and can send back messages to Node-RED.  
**For example:**  
```javascript
<div layout="row" layout-align="space-between">
  <p>The number is</p>
  <font color="{{((msg.payload || 0) % 2 === 0) ? 'green' : 'red'}}">
    {{(msg.payload || 0) % 2 === 0 ? 'even' : 'odd'}}
  </font>
</div>
```
Will display if the number received as`msg.payload`is even or odd. It will also change the color of the text to green if the number is even or red if odd.  
The next example shows how to set a unique id for your template, pick up the default theme colour, and watch for any incoming message.  
```javascript
<div id="{{'my_'+$id}}" style="{{'color:'+theme.base_color}}">Some text</div>
<script>
(function(scope) {
  scope.$watch('msg', function(msg) {
    if (msg) {
      // Do something when msg arrives
      $("#my_"+scope.$id).html(msg.payload);
    }
  });
})(scope);
</script>
```
Templates made in this way can be copied and remain independent of each other.  
**Sending a message:**  
```javascript
<script>
var value = "hello world";
// or overwrite value in your callback function ...
this.scope.action = function() { return value; }
</script>
<md-button ng-click="send({payload:action()})">
Click me to send a hello world
</md-button>
```
Will display a button that when clicked will send a message with the payload`'Hello world'`.  

**Using`msg.template`:**  
You can also define the template content via`msg.template`, so you can use external files for example.  
Template will be reloaded on input if it has changed.  
Code written in the Template field will be ignored when`msg.template`is present.  

The following icon fonts are available: [Material Design icon](https://klarsys.github.io/angular-material-icons/)*(e.g. 'check', 'close')*or a [Font Awesome icon](https://fontawesome.com/v4.7.0/icons/)*(e.g. 'fa-fire')*, or a [Weather icon](https://github.com/Paul-Reed/weather-icons-lite/blob/master/css_mappings.md). You can use the full set of Google material icons if you add 'mi-' to the icon name. e.g. 'mi-videogame_asset'.  
If a **Class** is specified, it will be added to the parent card. This way you can style the card and the elements inside it with custom CSS. The Class can be set at runtime by setting a`msg.className`string property.  

### 3.8. M300

#### 3.8.1. get sn
Get sn node.  
**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| For example: (Normal：02900123021600000815; abnormal：err) |     |

#### 3.8.2. get mac
Get mac node  

**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| For example: (Normal：D4AD203651BC; abnormal：err) |     |

#### 3.8.3. get iccid

Get iccid node. Reads the ICCID of the M300 device and outputs it in string format.  
**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| For example: (Normal：8986032047205253964; abnormal：err) |     |

#### 3.8.4. get imei
Get IMEI node. Reads the IMEI of the M300 device and outputs it in string format.

**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| For example: (Normal：866859037026521; abnormal：err) |     |

#### 3.8.5. edge get node
Read the value of a variable in the M300 device collection point table and output it in string format.  
**Inputs:**   Any type.  
**Outputs:**  
| Description           | Type   |
| --------------------- | ------ |
| payload               | string |
| Topic data point name | string |
| For example: (Normal：1; abnormal：err)                      |        |

#### 3.8.6. edge get nodes
Read the values of multiple variables in the M300 device collection point table and output them in string format.  

**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| For example: (Normal：1,0,12.3,45; abnormal：err) |     |

#### 3.8.7. edge set node
Set the value of a variable in the M300 device collection point table and output ok or err in string format.  
**Inputs:**  
| Description  | Type |
| --- | --- |
| payload | string, value |

**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| Normal: ok; abnormal: err) |     |

#### 3.8.8. serial config
Configure the parameters of serial port of the M300.  

**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| Normal: ok; abnormal: err |     |

#### 3.8.9. serial send
Send data through the serial port on the M300 device.  

**Inputs:**  
| Description  | Type |
| --- | --- |
| payload | buffer |

**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| Normal: ok; abnormal: err) |     |

#### 3.8.10. serial receive
Receive data through the serial port of the M300 device.  

**Inputs:**   Any type.  
**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| Normal: ok; abnormal: err |     |

Note: Only need to call it once when using it, and don't need to call it repeatedly.  

#### 3.8.11. sms send
Send text messages via the SIM card of M300 device.  

**Inputs:**  
| Description  | Type |
| --- | --- |
| payload |string |

**Outputs:**  
| Description  | Type |
| --- | --- |
| payload | string |
| Normal: ok; abnormal: err) |     |

Note: A SIM card needs to be installed when using it, and the SIM card needs to have the function of sending SMS.	  










