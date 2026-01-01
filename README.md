# AI Agent for Automated Incident Resolution

Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.

## 1. Introduction

In modern IT Operations (AIOps), the speed at which incidents are resolved is a critical metric (Mean Time To Resolution, or MTTR). Automating the resolution process for common incidents can dramatically improve system reliability and free up human engineers to focus on more complex problems.

This project provides a Jupyter Notebook that simulates an autonomous **AI Incident Response Agent**. This agent is designed to handle a common IT incident—a crashed web server application—from detection to resolution. It demonstrates how a single agent can use a variety of "tools" to interact with different IT systems to diagnose a problem, perform a fix, and document its actions.

## 2. The Solution Explained: A Multi-Tool Agent Workflow

This simulation demonstrates a sophisticated, tool-using AI agent. The agent is given a goal (resolve an incident) and orchestrates a series of actions using its specialized tools.

### 2.1 The Simulated Environment

To make the notebook self-contained, we simulate the key components of an IT environment:
*   **`SimulatedServer`:** A class that mimics a production server. It holds a state, such as whether its primary application process is `running` or `crashed`.
*   **`SimulatedITSM`:** A class that acts as a simple IT Service Management tool (like ServiceNow or Jira). It can create, store, and update incident tickets.
*   **Monitoring System:** A simple function that checks the server's health and automatically creates a new ticket in the ITSM system if a problem is detected.

### 2.2 The Agent's Toolkit

The `IncidentResponseAgent` is equipped with a set of tools that allow it to interact with its environment:

1.  **`run_diagnostics(server_id)`:** This tool "connects" to the specified server and returns a report on its status, such as whether the main application process is running.
2.  **`execute_remediation_script(server_id, script_name)`:** This tool simulates running a command on the server. For this scenario, it runs a `'restart_web_server'` script, which sets the server's application status back to `running`.
3.  **`update_itsm_ticket(incident_id, notes, status)`:** This tool interacts with the ITSM system to add resolution notes and change the ticket's status (e.g., from 'New' to 'Resolved').

### 2.3 The Agent's Chain of Thought

The notebook clearly shows the agent's logical, step-by-step reasoning process for handling an incident:

1.  **Detect:** A monitoring function finds an unhealthy server and creates a new incident ticket.
2.  **Acknowledge:** The `IncidentResponseAgent` polls the ITSM, finds the new, unassigned incident, and begins work.
3.  **Diagnose:** It uses the `run_diagnostics` tool to understand the root cause (a crashed process).
4.  **Plan:** Based on the diagnosis, the agent's internal logic selects the appropriate remediation script (`'restart_web_server'`).
5.  **Act:** It uses the `execute_remediation_script` tool to run the fix.
6.  **Verify:** It runs diagnostics *again* to confirm that the fix was successful and the application process is now running.
7.  **Document:** It uses the `update_itsm_ticket` tool to add detailed resolution notes and formally resolve the incident ticket.

## 3. How to Use the Notebook

### 3.1. Prerequisites

This project is self-contained and only requires standard Python libraries like `pandas` and `numpy`.

```bash
pip install pandas numpy
```

### 3.2. Running the Notebook

1.  Clone this repository:
    ```bash
    git clone https://github.com/shrikarak/incident-resolution-ai-agent.git
    cd incident-resolution-ai-agent
    ```
2.  Start the Jupyter server:
    ```bash
    jupyter notebook
    ```
3.  Open `incident_resolution_simulation.ipynb` and run the cells sequentially. The output will narrate the entire incident lifecycle as the AI agent performs each step.

## 4. Deployment and Customization

This simulation provides a blueprint for creating powerful, real-world AIOps agents.

1.  **Connect to Real Systems:** The core logic of the `IncidentResponseAgent` can be preserved, while the `AgentTools` are modified to make real API calls instead of interacting with simulations.
    *   The `run_diagnostics` and `execute_remediation_script` tools could be implemented using a library like **`paramiko`** to connect to servers via SSH.
    *   The `update_itsm_ticket` tool could be implemented using the **`requests`** library to interact with the REST APIs of ServiceNow, Jira, or PagerDuty.

2.  **Expand the Playbook:** The agent's simple `if/then` logic can be expanded into a larger "playbook." A more advanced agent might use a classification model or an LLM to select the correct remediation script from dozens of options based on the diagnostic output.

3.  **Create More Agents:** The system could be expanded into a true multi-agent system where a `DispatcherAgent` assigns incidents to different specialized `ResponderAgents` based on the incident type (e.g., database issue, network issue, application issue).
