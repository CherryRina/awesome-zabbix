## Table of Contents
1. awesome-zabbix Project
2. What is Zabbix
3. Components
4. Monitoring Types
5. Zabbix Workflow

## awesome-zabbix Project
As part of my learning process, this repository covers core Zabbix concepts, architecture, monitoring workflows, hands-on configurations, and automation techniques. It aims to build a strong foundation in infrastructure and application monitoring using Zabbix.

## What is Zabbix
Zabbix is a [[monitoring]] tool. it is mainly used for infrastructure and applications but can also monitor servers, networks and cloud resources.
It collects metrics and stores them inside a database for alerting and visualization.

## Components
#### Core Architecture
- **Server**: Core component - collects and stores data.
- **Proxy**: This component is optional, used for distributed monitoring.
- **Database**: Stores all configuration and historical data (metrics, events, triggers, trends).

#### Data Collection Components
- **Agents**: A lightweight binary installed on the host to be monitored. It collects system metrics and sends them to the Zabbix server or proxy using the Zabbix protocol.
	- **Active Agents**: Send data to server without being asked for it (port **TCP 10051**).
	- **Passive Agents**: Wait for the server to request data (port **TCP 10050**).
- **Item**: The basic unit of data collection in Zabbix. 
  Each item has: key, data type, collection intervals, type(agent, SNMP etc.)
	- **Trapper Item**: Collects data from the sender (not on its own).
- **Application**: A logical group of items, they help organize data in the frontend.

#### Tools
- **Sender**(zabbix_sender): When a metric is collected outside Zabbix's usual methods, `zabbix_sender` is used to manually push the data to the Zabbix server or proxy. This data is then received by a Trapper item, which stores it like any other metric.
- **Get**(zabbix_sender): Its a CLI tool that queries the agent for specific item values, mainly for debug purposes.
  When used its connected to a computer that then is connected to the component we want to debug. It queries a passive agent to retrieve a value.

#### Configuration and reusability
- **Templates**: Those are reusable sets of items, triggers, graphs, applications, macros... 
  You can use templates to avoid long manual work.
- **Macros**: Place holders that are used in items, triggers and actions.
	- **built-in macros** like {TRIGGER.STATUS}
	- **user macros** that look like this {$CPU_WARN}.
	They allow parameterization and reuse in templates and actions.

#### Problem Detection
- **Trigger**: Is a logical expression that evaluates the data collected by the items.
  It determines if something is a problem or not and according to that changes its status.
- **Action**: A rule that defines what to do when a trigger changes its state to PROBLEM.
- **Workflow**: A Trigger changes state to PROBLEM and if conditions are met an action is executed (run script/send email etc.)

## Monitoring Types
- **agent**: system metrics from Zabbix agent 
- **SNMP**: Network hardware like switches and routers
- **IPMI**: hardware level monitoring
- **External**: run shell/Python scripts
- **Web**: monitor HTTP endpoints
- **Trapper**: Accepts push data from scripts or APIs

## Zabbix workflow
1. Each monitored **host runs a Zabbix agent** (passive or active).
2. Each host has items defined that **describes what data** to collect.
3. **Items form into applications** for easier organization.
4. **Agent collects data** from hosts.
5. **Agent sends data** to server or proxy.
6. The **server stores the data** in the database and **evaluates triggers**.
7. If a **trigger condition** is met, its state changes to **PROBLEM**.
8. Based on the trigger, an **action** is executed.
9. All collected data and events are **visualized in dashboards, maps, graphs, and latest data views**.
