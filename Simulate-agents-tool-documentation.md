# Simulate agents tool

This tool is intended to simulate a given number of agents `n` or a given amount of EPS load `q` per queue.

It is important to note that the number of simulated agents and EPS load is conditioned by the resources of the host
where the script is executed.

As reference data we have the following:

In a `c5.xlarge` (4 vCPUs - 8 GB Memory):

- We can deploy 500 agents that send only keep-alives to the manager (agent mode).
- We can generate 7000 EPS (this value may vary depending on the queues) distributed among 7 agents
  (each agent sends 1000 EPS).

> **Note**: Every agent created with this script will have the keepalive and receive_messages module enabled by default.

## How it works

Its operation is based on handling a set of objects of the `Agent` class located in the
[agent_simulator.py](https://github.com/wazuh/wazuh-qa/blob/master/deps/wazuh_testing/wazuh_testing/tools/agent_simulator.py)
module.

This script abstracts from the complexity of its use by handling the following parameters:

| Parameter | Objective | Required | Default | Type |
|-----------|-----------|----------|---------|------|
| -a --manager | Manager IP address where the agent will connect | True | localhost | str |
| -n --agents | Number of agents to create and run | False | 1 | int |
| -o --os | Simulated agent operating system  | False | debian 8 | str |
| -p --protocol | Communication protocol  | False | TCP | str |
| -r --registration-address | Manager IP address where the agent will be registered  | False | None | str |
| -t --time | Time in seconds for the process simulation  | False | 60 | int |
| -v --version | Agent wazuh version  | False | 4.2.0 | str |
| -m --modules | Active module separated by whitespace  | False | ['keepalive', 'receive_messages'] | list(str) |
| -l --labels | Wazuh agent labels | False | None | list(str) |
| -s --modules-eps | Active module EPS separated by whitespace  | False | ['0', '0'] | list(str) |
| -f --fixed-message-size' | Size of all the agent modules messages (KB) | False | None | int |
| -b --balance-mode |Activate the balance mode. EPS will be distributed throughout all agents. | False | None | Flag |
| -i --balance-ratio | EPS/agent ratio. Can only be used if the parameter -b was specified | False | 1000 | int |
| -w --waiting-time | Waiting time in seconds between agent registration and the sending of events | False | 0 | int |

**Parameter restrictions**

- `-o --os`: Must be one of the following list:

  ```
  ["debian7", "debian8", "debian9", "debian10", "ubuntu12.04", "ubuntu14.04", "ubuntu16.04", "ubuntu18.04", "mojave"]
  ```

- `-m --modules`: Allowed modules:

  ```
  ["fim", "fim_integrity", "syscollector", "rootcheck", "sca", "hostinfo", "winevt", "logcollector" ]
  ```

- `-i --balance-ratio`: Only must be specified if `-b` parameter was too.

- `-l --labels`: Must have the following format

  ```
  label1:value1 label2:value2 label3:value3 ...
  ```

## Modes of use

**Create agents that only send keep-alives (agent mode)**

This mode is used when you want to observe how the manager behaves when he has `n` agents connected and active.

To use this mode, just select a number of agents `n`. It would be as follows:

```
simulate-agents -a <manager_ip> -n <number_of_agents>
```

---

**Create n agents and each one sends x EPS (hard EPS mode)**

This mode is used to create `n` agents that are reporting the same amount of EPS

For example, to create 3 agents and each send 500 EPS from `FIM` and 1000 from `logcollector`:

```
simulate-agents -a <manager_ip> -n 3 -m fim logcollector -s 500 1000
```

The result would be:

```
agent 1 --> 500 FIM EPS + 1000 logcollector EPS
agent 2 --> 500 FIM EPS + 1000 logcollector EPS
agent 3 --> 500 FIM EPS + 1000 logcollector EPS

Total --> 3 agents and 4500 EPS
```

>**Note**: t is necessary to take into account the possible limitations of the host where the script is launched, i.e.
the number of agents created and the total amount of EPS will depend on the resources allocated to the host where this
tool will be executed.

---

**Send q load distributed in a ratio value per agent (balanced mode)**

This mode is used when we want to send a certain total load, and we want to specify the maximum amount of load sent by
each agent. All the EPS of the specified modules will be distributed among different agents, which will be
self-calculated using the agent-EPS ratio (`-i` parameter).

It is important that to use this mode the `-b` flag is specified to indicate that the balanced mode will be used,
and `-i` to indicate the agent-EPS ratio (If the `-i` parameter is not specified, the default value will be used,
which is 1000EPS/agent)

For example, if we want to generate 500 EPS from `FIM` and 700 from `logcollector`, using a ratio of 500EPS/agent:

```
simulate-agents -a <manager_ip> -m fim logcollector -s 500 700 -b -i 500
```

The result would be:

```
agent 1 --> 500 FIM EPS
agent 2 --> 500 logcollector EPS
agent 3 --> 200 logcollector EPS

Total --> 3 agents and 1200 EPS
```

## How install this tool

In order to use this tool, first you need to clone the github repository

```
git clone https://github.com/wazuh/wazuh-qa.git
```

Next you have to install the python dependencies from the repository

```
cd wazuh-qa && pip install -r requirements.txt
```

Finally, you have to install the repository tools

```
cd deps/wazuh-testing && python3 setup.py install
```

After finishing, you can run the command `simulate-agents` to see the tool's help menu.

```
simulate-agents
```

>Note: If the  script could not be found, check that /usr/local/bin is added to your PATH variable

## Usage examples

- Create 500 agents who are only online and sending `keep-alives` reporting to a cluster with a load balancer

  ```
  simulate-agents -a <load_balancer_ip_or_dns> -n 500 -r <master_node_ip>
  ```

- Create 200 agents who are only online and sending `keep-alives`  with custom labels during 120 seconds

  ```
  simulate-agents -a <manager_ip> -n 200 -l label1:value1 label2:value2 label3:value3 -t 120
  ```

- Create 3 agents, each reporting 100 `FIM` EPS and 50 `rootcheck`, sending events of size 20KB.

  ```
  simulate-agents -a <manager_ip> -n 3 -m fim rootcheck -s 100 50 -f 20
  ```

- Generate a load of 3000 `FIM` EPS, 500 `logcollector`, 200 `syscollector` and 1500 `winevt` between agents with a
maximum ratio of 500EPS/agent

  ```
  simulate-agents -a <manager_ip> -m fim logcollector syscollector winevt -s 3000 500 200 1500 -b -i 500
  ```

- Register 500 agents and wait 30 seconds before connecting them (avoids overloading the manager if there are several
parallel processes trying to register more agents).

  ```
  simulate-agents -a <manager_ip> -n 500 -w 30
  ```
