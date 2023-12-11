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





















