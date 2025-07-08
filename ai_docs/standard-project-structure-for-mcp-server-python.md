

# **Standard Python Project Structure for a Model Context Protocol (MCP) Server**

## **Executive Summary**

This report delineates a comprehensive, standard project structure for a Python-based Model Context Protocol (MCP) Server. The proposed architecture prioritizes modularity, clear interfaces, and machine-readable metadata, ensuring optimal understanding and usability by human developers, AI agents, and Large Language Models (LLMs). Key recommendations include adopting the src layout for packaging clarity, implementing robust documentation and code quality standards, and integrating specialized modules for AI/ML models, MCP-specific tools, and advanced context management. By adhering to these principles, the MCP Server will be robust, maintainable, scalable, secure, and inherently more interpretable by intelligent systems.

A fundamental consideration throughout this architectural design is the requirement for the project structure to be comprehensible by humans, AI, and LLMs. While traditional software engineering emphasizes human readability, the need for AI/LLM understanding necessitates a focus on machine-parseable, structured information. Elements such as clear docstrings 1, explicit type hints 2, consistent naming conventions 1, and rigorously structured configuration files 4 are not merely beneficial practices for human developers; they constitute critical data points for AI and LLM agents that attempt to interpret or interact with the codebase. The

src layout, for instance, assists AI/LLMs by distinctly separating importable code from other project artifacts.7 This implies that standard software engineering best practices, when applied with precision and an awareness of machine-readability, inherently fulfill the requirement for AI/LLM comprehension. The design does not involve writing code

*for* AI, but rather writing *well-structured, well-documented* code that AI can then more readily parse and interpret. The subsequent sections detail how each structural and coding practice contributes to both human and machine comprehension, treating them as interconnected aspects of a single objective.

## **Introduction to Model Context Protocol (MCP) Servers**

### **Defining MCP: Bridging LLMs with External Tools and Data**

The Model Context Protocol (MCP), introduced by Anthropic in late 2024, represents a significant advancement in the integration of AI systems. It is an open standard specifically designed to bridge the gap between AI assistants, particularly Large Language Models (LLMs), and the diverse, data-rich ecosystems they need to navigate.8 This protocol provides a universal framework that connects LLMs with external tools and data sources, granting AI systems seamless access to relevant contexts.8 By offering a standardized approach for LLM applications to share contextual information, expose specialized tools and capabilities, and construct composable integrations, MCP effectively eliminates the fragmented and often inconsistent integrations that have historically challenged AI development.8

### **The Core Architecture: Hosts, Clients, Servers, and Their Interactions**

MCP operates on a secure and efficient client-host-server paradigm, built upon JSON-RPC 2.0 messages, which facilitates stateful sessions for the exchange of context and sampling operations.8

* **Hosts:** These entities are typically the LLM applications themselves, functioning as containers or coordinators for multiple client instances. Hosts are responsible for managing the lifecycle of these instances and enforcing crucial security policies, including permissions, user authorization, and consent requirements.8  
* **Clients:** Each client operates within a host and is tasked with capability negotiation and the orchestration of messages between itself and an MCP server. Clients are designed to maintain strict security boundaries, ensuring that resources belonging to one client cannot be accessed by another.8  
* **Servers:** These are the services that furnish context and capabilities to the AI systems.9 MCP Servers encapsulate external data sources, APIs, or other utilities, such as Customer Relationship Management (CRM) systems, Git repositories, or file systems. They define and expose three primary types of functionalities: "Resources" (contextual data for user or AI model consumption), "Prompts" (templated messages for users), and "Tools" (functions that the AI model can execute).8 Servers are mandated to adhere rigorously to the security constraints and user permissions enforced by the host.8

### **Why a Well-Structured Python Server is Critical for MCP Adoption**

The development of a well-structured Python server for MCP is of paramount importance. Such a structure ensures the reproducibility of AI environments, promotes standardization and collaboration across development teams, and facilitates secure, efficient access to diverse data and tools.8 This architectural rigor is indispensable for production-grade AI systems, as it directly contributes to maintainability, scalability, and testability.2 The Model Context Protocol Python SDK further streamlines the development process by offering advanced context management and intelligent routing capabilities.10

MCP servers are often conceptualized as the "AI ecosystem's toolboxes".8 This analogy underscores a critical architectural implication: by exposing consistent interfaces, these servers resolve the "N times M problem," where distinct integrations would otherwise be required for every AI application and every data source.8 A server, functioning as a toolbox, must possess a highly organized internal structure, with clearly labeled tools (modules and functions), easily accessible interfaces (APIs), and robust mechanisms to guarantee tool reliability (e.g., comprehensive testing and error handling). This directly translates into an architectural imperative for clear modularization 1, consistent naming 1, and well-defined API endpoints. The resolution of the "N times M problem" also implies that the server must be sufficiently generic to serve a variety of clients, thereby reinforcing the necessity for a standardized and extensible structure. Modular design, in this context, transcends mere coding preference; it becomes an architectural necessity for effective AI/LLM interaction within the MCP framework. The project structure must explicitly cater to this "toolbox" concept by incorporating dedicated, well-defined sections for

tools/ and models/, supported by a robust API layer that serves as the primary access point to these capabilities.

**Table 1: Key MCP Server Components and Their Purpose**

| Component | Purpose/Role within MCP | Relevance to Python Server |
| :---- | :---- | :---- |
| **Host** | LLM application; coordinates clients, manages lifecycle, enforces security (permissions, consent). | The Python server acts *as* a Server for a Host. Its design must anticipate and integrate with Host-enforced security and communication protocols. |
| **Client** | Connects within Host; negotiates capabilities, orchestrates messages with Server, maintains security boundaries. | The Python server *receives* requests from Clients. Its API design must align with Client expectations for tool invocation, resource requests, and prompt handling. |
| **Server** | Provides context and capabilities to AI systems; wraps data sources, APIs, utilities. | The Python server *is* the Server. Its internal structure directly reflects the organization and exposure of its Resources, Prompts, and Tools. |
| **Resources** | Contextual information and data for user or AI model use. | The Python server must implement modules to fetch, process, and serialize data as Resources, ensuring they are accessible and relevant to LLMs. |
| **Prompts** | Templated messages and workflows for users. | The Python server may host or generate dynamic Prompts, requiring structured templates and logic within its codebase. |
| **Tools** | Functions for the AI model to execute. | The Python server's core functionality often involves exposing Python functions as Tools, necessitating clear interfaces, input validation, and robust execution logic. |
| **Sampling** | Server-initiated agentic behaviors and recursive LLM interactions (client may offer this to server). | While primarily a client feature, the Python server's architecture should be capable of handling and responding to sampling requests if it offers agentic capabilities. |

## **Foundational Python Project Structure for Clarity**

### **Core Principles: Modularity, Separation of Concerns, High Cohesion, Low Coupling**

The bedrock of any robust and comprehensible Python project, including an MCP server, lies in adherence to core software engineering principles: modularity, separation of concerns, high cohesion, and low coupling. These principles are fundamental for creating applications that are not only maintainable and scalable but also inherently reusable and testable.1

**Modularity** involves the decomposition of large, complex programming tasks into smaller, independent, and reusable subtasks or modules.1 This approach simplifies the development process by enabling developers to focus on individual components in isolation. It significantly enhances maintainability, as changes or bug fixes can be localized to specific modules without affecting the entire system. Furthermore, modularity promotes code reusability across different parts of the project or even in future projects, reduces duplication of code, and helps prevent namespace conflicts by allowing each module to define its own distinct namespace.1

**Separation of Concerns** dictates that each module or component within the project should possess a single, well-defined responsibility. This prevents the intertwining of functionalities, which can lead to complex dependencies and make the codebase difficult to manage and understand.2 When a module is responsible for only one aspect of the system, its purpose becomes immediately clear to both human developers and automated analysis tools.

**High Cohesion** ensures that related functionality is logically grouped together within a single module.2 This principle fosters a natural organization where all elements necessary for a specific task reside in one place, making the module self-contained and easier to comprehend.

Conversely, **Low Coupling** minimizes the dependencies between modules.2 This means that modifications within one module should have minimal, if any, impact on other modules. Such independence is crucial for large-scale projects and collaborative development environments, as it reduces the likelihood of introducing unintended side effects and simplifies the process of integrating changes from multiple team members.1

The application of these principles naturally creates well-defined boundaries for functionalities. An MCP server, by design, exposes "Tools" and "Resources" to LLMs.8 For an LLM to effectively utilize these capabilities, it must comprehend their precise boundaries, expected inputs, and anticipated outputs. When a "tool" is implemented as a cohesive, low-coupling module, its function is isolated and predictable. This clarity allows the LLM to more reliably invoke the tool and interpret its results. In contrast, a monolithic or highly coupled codebase obscures the identification and utilization of specific functionalities as "tools" due to interwoven logic and ambiguous responsibilities. Therefore, modular design is not merely a coding preference; it is an architectural imperative for effective AI/LLM interaction within the MCP framework, as it directly translates to the clarity and reliability of exposed capabilities.

### **Recommended Directory Layout**

A consistent and logical directory layout is paramount for project navigability, maintainability, and scalability. The following structure is recommended for an MCP Server, incorporating best practices for human comprehension, efficient development workflows, and machine-readability for AI/LLM systems.

my-mcp-server/  
├── src/  
│   └── my\_mcp\_server/  
│       ├── \_\_init\_\_.py  
│       ├── \_\_main\_\_.py  
│       ├── server.py  
│       ├── models/  
│       │   ├── \_\_init\_\_.py  
│       │   ├── my\_llm\_model.py  
│       │   └── model\_loader.py  
│       ├── tools/  
│       │   ├── \_\_init\_\_.py  
│       │   ├── filesystem\_tool.py  
│       │   ├── database\_tool.py  
│       │   └── web\_fetch\_tool.py  
│       ├── context/  
│       │   ├── \_\_init\_\_.py  
│       │   ├── context\_manager.py  
│       │   └── persistence.py  
│       ├── config/  
│       │   ├── \_\_init\_\_.py  
│       │   └── settings.py  
│       └── utils/  
│           ├── \_\_init\_\_.py  
│           └── helpers.py  
├── tests/  
│   ├── unit/  
│   │   ├── test\_models.py  
│   │   └── test\_tools.py  
│   ├── integration/  
│   │   └── test\_server\_integration.py  
│   └── performance/  
│       └── test\_load.py  
├── docs/  
│   ├── README.md  
│   ├── api\_spec.json  (Auto-generated OpenAPI/Swagger)  
│   └── usage\_guide.md  
├── data/ (Optional: Sample data, pre-trained model artifacts)  
│   ├── sample\_data.json  
│   └── pre\_trained\_model.bin  
├── configs/ (Environment-specific configuration files)  
│   ├── development.yaml  
│   ├── production.yaml  
│   └── testing.yaml  
├── venv/ (Virtual environment directory \- typically.gitignore'd)  
├──.gitignore  
├── pyproject.toml  
└── README.md  
└── LICENSE

#### **my-mcp-server/ (Root Project Directory)**

This is the top-level container for the entire project, establishing a clear boundary for the codebase. Its primary function is to serve as the entry point for developers and automated systems alike, providing a high-level overview of the project's contents.

#### **src/ (Source Code \- leveraging src layout for packaging and import clarity)**

The src layout is highly recommended for Python projects, particularly those intended for distribution as packages or deployed in complex environments. This structure places all importable Python code into a dedicated src/ subdirectory.7

The adoption of the src layout offers several advantages. It effectively prevents the accidental usage of in-development code by ensuring that the installed copy of the package is utilized during runtime, rather than potentially conflicting local files.7 This is a critical safeguard, as the Python interpreter includes the current working directory as the first item on its import path. Without the

src layout, an import package in the current working directory with the same name as an installed package would be used, potentially leading to subtle misconfigurations and issues where files are inadvertently excluded from a distribution.7 Furthermore, this layout enforces that editable installations (e.g.,

pip install \-e.) only import files explicitly intended to be part of the package. This prevents non-code project files, such as README.md or tox.ini, from being inadvertently added to the Python import path, which could cause imports to function in development but fail in production.7 Such consistency is invaluable for reliable behavior across development, testing, and production environments. The

create-mcp-server tool, a reference implementation for MCP projects, already employs this structure 11, reinforcing its status as a best practice in this domain.

A minor consideration of the src layout is that running command-line interfaces directly from the source tree typically necessitates the project's installation, often in editable mode.7 However, common workarounds, such as invoking the package via

python \-m my\_mcp\_server, are often preferred for production execution and are well-supported.12

The src layout provides a distinct advantage for AI/LLM consumption by establishing a clear "contract" regarding what constitutes the "public API" of the Python package. The primary benefit of this layout—preventing accidental imports and ensuring consistent behavior between development and deployed environments 7—translates directly into a more reliable understanding for an AI or LLM. If an LLM is tasked with generating code or interacting with the server, knowing that only code within

src/ is genuinely importable and stable helps it avoid generating invalid or brittle interactions. This structure provides an unambiguous boundary for what the "server" represents from an importable code perspective, thereby reducing ambiguity in an AI's comprehension of the codebase's exposed functionalities. Documenting the use of the src layout clearly within the README.md and potentially in machine-readable metadata (e.g., pyproject.toml comments or a separate schema) would further enhance AI/LLM understanding of the project's internal structure and import rules, making the project more "AI-friendly."

#### **my\_mcp\_server/ (Python Package for the Server)**

This subdirectory represents the actual Python package containing the server's core logic. It is essential for this directory to contain an \_\_init\_\_.py file to be recognized as an importable package by Python.4

* **\_\_init\_\_.py**: This special file serves as a signal to Python that the directory it resides in is a package.4 It can be an empty file or contain package-level initialization code, such as setting up global logging configurations or importing key submodules to facilitate easier access from outside the package.  
* **\_\_main\_\_.py**: This file provides a command-line entry point for the package, allowing it to be executed directly using the python \-m my\_mcp\_server command.11 Its design should be minimal, primarily importing and executing the main server application logic contained in  
  server.py.12  
* **server.py**: This file encapsulates the main server application logic. It typically integrates with a web framework like FastAPI to define and manage API endpoints.14 This file acts as the central orchestrator, responsible for handling incoming MCP requests, validating them, and dispatching them to the appropriate internal modules for processing.  
* **models/ (Subpackage for ML models and their loading/inference logic)**: This dedicated subpackage is designed to house all machine learning models (e.g., PyTorch, TensorFlow, Scikit-learn) that the MCP server might expose as "Tools" or utilize internally for its operations.14 This separation is crucial for effective management of model versions, handling model persistence, and implementing dynamic loading strategies. The directory should contain submodules for different model types or specific models, along with their respective loading and inference interfaces.  
  An MCP server's primary function is to provide context and tools to LLMs 8, which frequently involves serving ML models for inference.16 By placing models within a dedicated  
  models/ subpackage, the structure explicitly communicates to both human developers and AI/LLM agents that this is the repository of the system's "intelligence." The capability to dynamically load models 19 based on runtime context or specific LLM requests (e.g., an LLM requesting a particular version of a model) is a key feature for building scalable and flexible MCP servers. This modularity facilitates efficient model versioning 20, enables A/B testing of different model implementations 20, and supports optimized resource allocation.18 These capabilities allow the server to adapt seamlessly to varying demands and model updates without requiring service downtime. Consequently, the  
  models/ directory should contain not only the model artifacts themselves but also the code responsible for loading them (e.g., model\_loader.py) and defining their inference interfaces. This explicit structure enhances an LLM's ability to identify and interact with specific model capabilities, improving its overall utility.  
* **tools/ (Subpackage for MCP-defined tools/capabilities)**: This subpackage contains the Python implementations of the "Tools" that the MCP server exposes to LLMs.8 Each tool should ideally be implemented as a separate module or class within this directory, strictly adhering to the Single Responsibility Principle.2 This structural choice ensures that each tool's functionality is isolated and clearly defined.  
  LLMs are designed to "execute" tools exposed by an MCP server.9 For this tool-use to be effective and reliable, the tools must possess well-defined interfaces—including clear inputs, outputs, and predictable side effects—that an LLM can readily understand and interact with. Placing these implementations in a dedicated  
  tools/ directory, complemented by clear docstrings and type hints, directly supports this requirement. The directory structure itself, combined with consistent coding standards, becomes an integral part of the "API" for the LLM, enabling it to infer available functionalities and their proper invocation. This organization aligns perfectly with the "toolbox" analogy for MCP servers 8, where each tool is distinct and easily identifiable. Thus, each file or module within  
  tools/ should correspond to a distinct MCP tool, with its public functions clearly annotated and described for LLM consumption. This explicit organization and internal documentation are paramount for effective AI-driven tool utilization.  
* **context/ (Subpackage for MCP context management logic)**: This critical subpackage is responsible for the dynamic creation, multi-layered tracking, serialization, and intelligent routing of contextual information for LLMs.10 It may encompass logic for fetching context from various external sources, transforming raw data into a usable format, maintaining context state across multiple interactions, and managing caching mechanisms.  
  The core value proposition of MCP is its ability to provide "context" to LLMs, which is essential for them to generate relevant and informed responses.9 This context is not static; it is dynamic, often multi-layered, and requires active management.10 The presence of a  
  context/ subpackage implies a dedicated pipeline for context processing. This could involve complex data retrieval, sophisticated data transformation, efficient caching strategies 10, and even integration with long-term memory solutions such as vector stores.22 The concept of "intelligent routing" 10 suggests that this module must be adept at determining  
  *how* context is delivered to different models or parts of the system, optimizing for relevance and efficiency. Consequently, this module will likely contain intricate logic, potentially involving asynchronous operations 10 and external data persistence mechanisms, to ensure that context is consistently fresh, relevant, and efficiently delivered to the LLM. Its design must prioritize performance and reliability to prevent it from becoming a bottleneck for LLM interaction.  
* **config/ (Application configuration management)**: This subdirectory is exclusively dedicated to managing application settings, parameters, and environment-specific configurations (e.g., development, testing, production).4 It should leverage structured formats such as YAML, JSON, or INI for both human readability and machine parsing.4 Furthermore, it is imperative to integrate with environment variables for handling sensitive data like API keys, ensuring that such credentials are not hardcoded into the codebase.3  
  Configuration files abstract operational details from the core code, enhance security, and enable straightforward updates without modifying source code.5 For AI/LLMs, well-structured configuration files (e.g., YAML, JSON) function as a machine-readable blueprint of the server's operational parameters. An LLM could potentially parse these files to understand, for instance, which external services the server connects to, or what configurable behaviors are available (though sensitive values themselves would be securely managed outside the repository). This capability is crucial for automated deployment, troubleshooting, and even self-correction by advanced AI agents. The use of libraries like Pydantic for defining settings classes 24 further enforces type safety, which is highly valuable for machine parsing and validation. Therefore, the  
  config/ directory should contain clear, human-readable, and machine-parseable configuration files, potentially with schema definitions or examples, to facilitate automated understanding and management by AI/LLM systems.  
* **utils/ (General utility functions)**: This subpackage contains reusable helper functions that do not logically fit into other specific domains. Examples include common data transformations, general validation routines, or centralized logging setup utilities. Segregating these utilities prevents code duplication across the project and significantly improves overall maintainability.

#### **Top-Level Files and Directories**

* **tests/ (Comprehensive unit, integration, and functional tests)**: This top-level directory is fundamental for automated quality assurance, enabling the early detection of bugs and ensuring that the code functions as expected under various conditions.1 It should contain a well-organized suite of unit tests (for individual functions and modules), integration tests (for interactions between components), and performance tests (to assess scalability under load).25 Pytest is a widely recommended and powerful testing framework for Python projects.2  
  Tests define the expected behavior of the code.1 For human developers, they serve as executable examples, demonstrating how to use the code and what outputs to anticipate from specific functions or modules. For AI/LLMs, tests can be parsed as executable specifications. An LLM could potentially analyze test cases to infer the precise functionality, identify edge cases, and understand the expected outputs of a module or tool, especially when docstrings are insufficient or ambiguous. This process represents a powerful form of "learning by example" for the AI, enabling it to better comprehend the system's operational logic. Consequently, well-written, comprehensive tests with clear assertions are not merely for quality assurance; they constitute a valuable form of machine-readable documentation that can significantly enhance AI/LLM understanding and interaction with the codebase.  
* **docs/ (Project documentation, API specifications, usage guides)**: This directory serves as the centralized repository for all project documentation.1 It should include the  
  README.md (as detailed below), detailed API documentation (potentially auto-generated by tools like FastAPI's Swagger UI 14), design documents, and comprehensive usage guides for both developers and end-users.  
  While code structure and tests provide implicit signals, explicit documentation remains the most direct and comprehensive method to communicate intent, functionality, and usage to both human developers and AI/LLM systems. For LLMs, well-structured markdown (README.md), clear API specifications (e.g., OpenAPI/Swagger JSON), and detailed explanations of tools and context management are crucial for effective interaction. This is where the human-readable and AI-readable aspects of the project converge most strongly. Tools like FastAPI's auto-generated Swagger UI 14 exemplify documentation that is inherently machine-readable (via JSON schema) and simultaneously human-friendly, making it ideal for AI consumption. Therefore, significant investment in clear, consistent, and structured documentation is warranted. Consideration should be given to tools that generate machine-readable formats (e.g., OpenAPI/Swagger for APIs) to maximize AI/LLM comprehension and enable automated interactions.  
* **data/ (Optional: Sample data, pre-trained model artifacts)**: This is an optional directory for storing sample datasets, pre-trained model artifacts, or other static data assets required by the application for development or testing purposes.4 Its inclusion helps maintain a clean and focused main source code directory.  
* **configs/ (Environment-specific configuration files)**: This dedicated directory houses various environment-specific configuration files (e.g., development.yaml, production.yaml, testing.yaml).4 This separation allows for straightforward management of different settings across various deployment stages without requiring modifications to the core application code.  
* **venv/ (Virtual environment directory \- typically .gitignored)**: This directory contains the isolated Python virtual environment for the project.1 It should always be explicitly excluded from version control using a  
  .gitignore file to prevent the accidental commitment of large, environment-specific files.3  
* **.gitignore**: This file specifies intentionally untracked files that Git should ignore. It is crucial for preventing sensitive information (e.g., .env files 3) and large, auto-generated files (e.g., virtual environments 3) from being committed to the repository.  
* **pyproject.toml (Modern project metadata, dependencies, build system)**: This file represents the modern standard for Python project configuration, centralizing project metadata, dependencies, and build system definitions. It effectively supersedes older setup.py files.1 Tools such as Poetry or  
  uv manage dependencies and project creation based on the specifications within this file.2  
  The pyproject.toml file centralizes critical project metadata, dependencies, and build system configuration.1 For an AI/LLM, this file serves as the primary manifest of the project. It explicitly communicates to the AI the project's name, version, required dependencies, and instructions for building or installing it. This information is indispensable for automated setup, efficient dependency resolution, and a comprehensive understanding of the project's ecosystem, thereby enabling AI-driven development tools to interact seamlessly. Therefore, meticulous maintenance of  
  pyproject.toml to include all relevant project metadata is crucial. Its structured format makes it highly valuable for automated parsing and interpretation by AI/LLM systems.  
* **README.md (Project overview, setup, quick start, philosophy)**: This file is the primary entry point for any entity, human or AI, encountering the project repository.1 It should clearly articulate the project's purpose, provide concise installation and usage instructions, and outline the project's underlying philosophy.11 For an MCP server, it is particularly important to highlight its role within the broader AI ecosystem.  
  The README.md is often the first file a human or AI/LLM processes when exploring a new repository.1 Its clarity and completeness directly influence how quickly and accurately an AI/LLM can "onboard" to the project. A well-structured  
  README with clear headings, illustrative code examples, and explicit instructions (such as the create-mcp-server philosophy emphasizing "Zero Configuration," "Best Practices," and "Batteries Included" 11) provides a high-level semantic understanding that guides further exploration and interaction. It functions as a concise summary for initial AI comprehension. Consequently, the  
  README.md should be treated as a critical piece of user (and AI) documentation, not merely an afterthought. Its quality directly correlates with the ease of adoption and integration for both human developers and automated systems.  
* **LICENSE (Software license)**: This legal document defines the terms and conditions under which the software project can be used, modified, and distributed.4 Its inclusion is essential for any open-source project, clarifying intellectual property rights and usage permissions.

## **Sample Files for the Project Template**

Here are sample contents for the key files and directories to help you get started with your MCP Server template.

### **my-mcp-server/pyproject.toml**

Ini, TOML

\[project\]  
name \= "my-mcp-server"  
version \= "0.1.0"  
description \= "A Model Context Protocol (MCP) server for AI/LLM integration."  
authors \= \[{ name \= "Your Name", email \= "your.email@example.com" }\]  
dependencies \= \[  
    "fastapi\>=0.111.0",  
    "uvicorn\[standard\]\>=0.30.1",  
    "pydantic-settings\>=2.3.4",  
    "python-dotenv\>=1.0.0",  
    "mcp\[cli\]\>=1.4.0", \# Core MCP SDK dependency \[26\]  
    "scikit-learn\>=1.5.0", \# Example ML library \[14\]  
    "pandas\>=2.2.2", \# Example data handling library \[14\]  
requires-python \= "\>=3.10"  
readme \= "README.md"  
license \= { text \= "MIT" }

\[project.scripts\]  
my-mcp-server \= "my\_mcp\_server.\_\_main\_\_:main" \# Defines CLI entry point \[2\]

\[build-system\]  
requires \= \["poetry-core\>=1.0.0"\] \# Or "hatchling" if using Hatch  
build-backend \= "poetry.core.masonry.api" \# Or "hatchling.build"

### **my-mcp-server/README.md**

# **My MCP Server**

This repository contains a template for building a Model Context Protocol (MCP) server using Python. The server is designed to provide contextual information and expose tools to Large Language Models (LLMs) in a standardized, secure, and scalable manner.

## **Project Structure**

The project follows a src layout for clear separation of source code and other project artifacts. Key directories include:

* src/my\_mcp\_server/: The main Python package for the server.  
  * server.py: Core FastAPI application logic.  
  * models/: Machine learning models and their loading/inference logic.  
  * tools/: Implementations of MCP-defined tools/capabilities.  
  * context/: Logic for dynamic context management.  
  * config/: Application configuration settings.  
  * utils/: General utility functions.  
* tests/: Comprehensive unit, integration, and performance tests.  
* docs/: Project documentation and API specifications.  
* configs/: Environment-specific configuration files (e.g., development.yaml).

## **Getting Started**

### **1\. Setup Virtual Environment and Install Dependencies**

It is highly recommended to use uv for fast and reliable package management.  
If you don't have uv installed:bash  
curl \-LsSf https://astral.sh/uv/install.sh | sh \# Mac/Linux

# **For Windows (PowerShell):**

# **powershell \-ExecutionPolicy ByPass \-c "irm [https://astral.sh/uv/install.ps1](https://astral.sh/uv/install.ps1) | iex"**

Navigate to the project root and initialize \`uv\`:  
\`\`\`bash  
cd my-mcp-server/  
uv init  
uv venv  
source.venv/bin/activate \# On Windows:.venv\\Scripts\\activate  
uv sync \--dev \--all-extras

Alternatively, using pip:

Bash

python \-m venv.venv  
source.venv/bin/activate \# On Windows:.venv\\Scripts\\activate  
pip install \-e. \# Install in editable mode  
pip install \-r requirements.txt \# If you prefer a requirements.txt

### **2\. Configure Environment Variables**

Create a .env file in the project root for sensitive configurations:

\#.env  
MCP\_SERVER\_HOST="0.0.0.0"  
MCP\_SERVER\_PORT=8000  
\# Add any other sensitive API keys or credentials here

Remember to add .env to your .gitignore file.

### **3\. Run the Server**

Bash

uv run my-mcp-server \# Using uv  
\# Or directly with python  
python \-m my\_mcp\_server

The server will typically run on http://0.0.0.0:8000. You can access the interactive API documentation (Swagger UI) at http://0.0.0.0:8000/docs.

## **Development**

### **Running Tests**

Bash

pytest tests/

### **Code Quality**

This project adheres to PEP 8 style guidelines.

* **Linting:** ruff check src/  
* **Formatting:** black src/

## **License**

This project is licensed under the MIT License. See the LICENSE file for details.

\#\#\# \`my-mcp-server/LICENSE\`

MIT License

Copyright (c) 2025 Your Name

Permission is hereby granted, free of charge, to any person obtaining a copy  
of this software and associated documentation files (the "Software"), to deal  
in the Software without restriction, including without limitation the rights  
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell  
copies of the Software, and to permit persons to whom the Software is  
furnished to do so, subject to the following conditions:  
The above copyright notice and this permission notice shall be included in all  
copies or substantial portions of the Software.  
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR  
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,  
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE  
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER  
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,  
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE  
SOFTWARE.

\#\#\# \`my-mcp-server/.gitignore\`

# **Python**

pycache/  
\*.pyc  
\*.pyo  
\*.pyd  
.Python  
env/  
venv/  
.venv/  
\*.egg-info/  
.pytest\_cache/  
.mypy\_cache/  
.ruff\_cache/  
.ipynb\_checkpoints/  
.env \# 3

# **Logs**

\*.log  
logs/

# **Data**

data/  
\*.db  
\*.sqlite3  
\*.pkl \# 15

\*.bin  
\*.pt  
\*.h5

# **OS generated files**

.DS\_Store  
Thumbs.db

\#\#\# \`my-mcp-server/src/my\_mcp\_server/\_\_init\_\_.py\`

\`\`\`python  
\# src/my\_mcp\_server/\_\_init\_\_.py  
"""  
My MCP Server Package.

This package contains the core logic for the Model Context Protocol (MCP) server.  
It includes modules for server setup, AI models, MCP tools, context management,  
configuration, and utilities.  
"""

import logging  
from.config.settings import settings

\# Configure basic logging for the package \[27\]  
\# For production, consider more advanced configurations (e.g., structured logging, log rotation, centralized logging) \[5, 6, 27\]  
logging.basicConfig(  
    level=settings.LOG\_LEVEL,  
    format='%(asctime)s \- %(name)s \- %(levelname)s \- %(message)s',  
    datefmt='%Y-%m-%d %H:%M:%S'  
)

logger \= logging.getLogger(\_\_name\_\_)  
logger.info(f"Initializing {settings.APP\_NAME} v{settings.APP\_VERSION}")

\# You can import key components here to make them easily accessible  
\# from my\_mcp\_server import server  
\# from my\_mcp\_server.models import model\_loader  
\# from my\_mcp\_server.tools import filesystem\_tool

### **my-mcp-server/src/my\_mcp\_server/\_\_main\_\_.py**

Python

\# src/my\_mcp\_server/\_\_main\_\_.py  
"""  
Command-line entry point for the My MCP Server application.

This file allows the package to be executed directly using \`python \-m my\_mcp\_server\`.  
It loads configuration and starts the Uvicorn server.  
"""

import uvicorn  
import os  
from dotenv import load\_dotenv \# For loading.env file \[26\]  
from my\_mcp\_server.config.settings import settings  
from my\_mcp\_server.server import app  
import logging

\# Load environment variables from.env file \[26\]  
load\_dotenv()

logger \= logging.getLogger(\_\_name\_\_)

def main():  
    """  
    Main function to run the MCP server.  
    """  
    logger.info(f"Starting {settings.APP\_NAME} server...")  
    logger.info(f"Server will listen on {settings.SERVER\_HOST}:{settings.SERVER\_PORT}")

    \# Uvicorn server configuration \[28\]  
    uvicorn.run(  
        app,  
        host=settings.SERVER\_HOST,  
        port=settings.SERVER\_PORT,  
        log\_level=settings.LOG\_LEVEL.lower(), \# Uvicorn expects lowercase log level  
        reload=settings.DEBUG\_MODE \# Enable reload in debug mode  
    )

if \_\_name\_\_ \== "\_\_main\_\_":  
    main()

### **my-mcp-server/src/my\_mcp\_server/server.py**

Python

\# src/my\_mcp\_server/server.py  
"""  
Main FastAPI application for the Model Context Protocol (MCP) server.

This module defines the API endpoints for interacting with the MCP server,  
including health checks, model inference, and tool execution.  
"""

from fastapi import FastAPI, HTTPException, status  
from pydantic import BaseModel  
import logging  
from my\_mcp\_server.config.settings import settings  
from my\_mcp\_server.models.model\_loader import get\_example\_model  
from my\_mcp\_server.tools.filesystem\_tool import read\_file\_content  
from my\_mcp\_server.context.context\_manager import get\_current\_context  
from my\_mcp\_server.utils.helpers import generate\_unique\_id

logger \= logging.getLogger(\_\_name\_\_)

\# Initialize FastAPI application  
app \= FastAPI(  
    title=settings.APP\_NAME,  
    version=settings.APP\_VERSION,  
    description="A Python-based Model Context Protocol (MCP) Server.",  
    debug=settings.DEBUG\_MODE  
)

\# \--- API Models (Pydantic) \---  
class PredictionRequest(BaseModel):  
    """  
    Represents a request for a model prediction.  
    """  
    input\_data: list\[float\]  
    model\_name: str \= "default\_model"

class PredictionResponse(BaseModel):  
    """  
    Represents a response containing a model prediction.  
    """  
    prediction: int  
    model\_used: str

class ToolExecutionRequest(BaseModel):  
    """  
    Represents a request to execute an MCP tool.  
    """  
    tool\_name: str  
    parameters: dict

class ToolExecutionResponse(BaseModel):  
    """  
    Represents a response from an MCP tool execution.  
    """  
    tool\_name: str  
    result: dict

\# \--- Server Lifecycle Events \---  
@app.on\_event("startup")  
async def startup\_event():  
    """  
    Handles server startup events.  
    Loads models and initializes resources.  
    """  
    logger.info("Server startup initiated.")  
    try:  
        \# Example: Load a model on startup \[14\]  
        global loaded\_model  
        loaded\_model \= get\_example\_model()  
        logger.info(f"Successfully loaded model: {loaded\_model.\_\_class\_\_.\_\_name\_\_}")  
    except Exception as e:  
        logger.error(f"Failed to load model on startup: {e}")  
        \# Depending on criticality, you might want to raise an exception to prevent startup  
        \# raise RuntimeError("Failed to initialize critical components.") from e

@app.on\_event("shutdown")  
async def shutdown\_event():  
    """  
    Handles server shutdown events.  
    Performs cleanup operations.  
    """  
    logger.info("Server shutdown initiated. Performing cleanup...")  
    \# Example: Close database connections, release resources  
    logger.info("Cleanup complete.")

\# \--- Health Check Endpoint \---  
@app.get("/health", summary="Health check endpoint", response\_model=dict)  
async def health\_check():  
    """  
    Checks the health and status of the MCP server.  
    """  
    logger.debug("Health check requested.")  
    return {"status": "healthy", "version": settings.APP\_VERSION, "app\_name": settings.APP\_NAME}

\# \--- Model Inference Endpoint \---  
@app.post("/predict", summary="Perform model inference", response\_model=PredictionResponse)  
async def predict(request: PredictionRequest):  
    """  
    Receives input data and returns a prediction from a specified model.  
    """  
    logger.info(f"Prediction request received for model: {request.model\_name}")  
    try:  
        \# In a real application, you would select the model based on request.model\_name  
        \# and handle different model types. For this template, we use the pre-loaded example.  
        if not hasattr(globals(), 'loaded\_model') or loaded\_model is None:  
            raise HTTPException(  
                status\_code=status.HTTP\_503\_SERVICE\_UNAVAILABLE,  
                detail="Model not loaded. Server might be initializing or encountered an error."  
            )

        \# Perform inference (dummy example) \[15\]  
        \# Ensure input\_data matches expected model input shape  
        prediction\_result \= loaded\_model.predict(\[request.input\_data\])  
        logger.info(f"Prediction made: {prediction\_result}")

        return PredictionResponse(prediction=int(prediction\_result), model\_used=request.model\_name)  
    except Exception as e:  
        logger.error(f"Error during prediction: {e}", exc\_info=True)  
        raise HTTPException(  
            status\_code=status.HTTP\_500\_INTERNAL\_SERVER\_ERROR,  
            detail=f"Prediction failed: {e}"  
        )

\# \--- MCP Tool Execution Endpoint \---  
@app.post("/tool/execute", summary="Execute an MCP tool", response\_model=ToolExecutionResponse)  
async def execute\_tool(request: ToolExecutionRequest):  
    """  
    Executes a specified MCP tool with given parameters.  
    """  
    logger.info(f"Tool execution request received for tool: {request.tool\_name}")  
    try:  
        \# In a real application, you would dynamically dispatch to the correct tool  
        \# based on request.tool\_name.  
        if request.tool\_name \== "read\_file":  
            file\_path \= request.parameters.get("path")  
            if not file\_path:  
                raise HTTPException(status\_code=status.HTTP\_400\_BAD\_REQUEST, detail="Missing 'path' parameter for read\_file tool.")  
            content \= read\_file\_content(file\_path)  
            result \= {"content": content, "file\_path": file\_path}  
        else:  
            raise HTTPException(status\_code=status.HTTP\_404\_NOT\_FOUND, detail=f"Tool '{request.tool\_name}' not found.")

        logger.info(f"Tool '{request.tool\_name}' executed successfully.")  
        return ToolExecutionResponse(tool\_name=request.tool\_name, result=result)  
    except HTTPException:  
        raise \# Re-raise FastAPI HTTPExceptions  
    except Exception as e:  
        logger.error(f"Error executing tool '{request.tool\_name}': {e}", exc\_info=True)  
        raise HTTPException(  
            status\_code=status.HTTP\_500\_INTERNAL\_SERVER\_ERROR,  
            detail=f"Tool execution failed: {e}"  
        )

\# \--- Context Retrieval Endpoint (Example) \---  
@app.get("/context/current", summary="Retrieve current context", response\_model=dict)  
async def get\_context():  
    """  
    Retrieves the current contextual information managed by the server.  
    """  
    logger.info("Context retrieval request received.")  
    try:  
        current\_context \= get\_current\_context()  
        logger.debug("Current context retrieved successfully.")  
        return {"context": current\_context}  
    except Exception as e:  
        logger.error(f"Error retrieving context: {e}", exc\_info=True)  
        raise HTTPException(  
            status\_code=status.HTTP\_500\_INTERNAL\_SERVER\_ERROR,  
            detail=f"Failed to retrieve context: {e}"  
        )

\# \--- Example of a simple custom endpoint \---  
@app.get("/greet/{name}", summary="Greet a user")  
async def greet\_user(name: str):  
    """  
    A simple endpoint to greet a user by name.  
    """  
    logger.info(f"Greeting request for: {name}")  
    unique\_id \= generate\_unique\_id()  
    return {"message": f"Hello, {name}\! Your unique ID is {unique\_id}"}

### **my-mcp-server/src/my\_mcp\_server/models/\_\_init\_\_.py**

Python

\# src/my\_mcp\_server/models/\_\_init\_\_.py  
"""  
This subpackage contains machine learning models and their associated logic  
for loading, inference, and management.  
"""

### **my-mcp-server/src/my\_mcp\_server/models/my\_llm\_model.py**

Python

\# src/my\_mcp\_server/models/my\_llm\_model.py  
"""  
Defines a dummy machine learning model for demonstration purposes.  
In a real application, this would be a trained model (e.g., Scikit-learn, PyTorch, TensorFlow).  
"""

import numpy as np  
import logging  
from sklearn.ensemble import RandomForestClassifier \# Example ML model \[15\]

logger \= logging.getLogger(\_\_name\_\_)

class DummyClassifier:  
    """  
    A simple dummy classifier that simulates a machine learning model.  
    It always returns a fixed prediction or a random one for demonstration.  
    """  
    def \_\_init\_\_(self):  
        self.is\_trained \= False  
        self.model \= None  
        logger.info("DummyClassifier initialized.")

    def train(self, X: list\[list\[float\]\], y: list\[int\]):  
        """  
        Simulates training a simple RandomForestClassifier.  
        """  
        logger.info("Simulating model training...")  
        \# In a real scenario, X and y would be actual training data.  
        \# For this dummy, we'll just create a simple model.  
        if len(X) \> 0 and len(y) \> 0:  
            self.model \= RandomForestClassifier(random\_state=42)  
            self.model.fit(X, y)  
            self.is\_trained \= True  
            logger.info("DummyClassifier training complete.")  
        else:  
            logger.warning("No data provided for training DummyClassifier.")  
            self.is\_trained \= False

    def predict(self, input\_data: list\[list\[float\]\]) \-\> list\[int\]:  
        """  
        Simulates making a prediction.  
        If trained, uses the model; otherwise, returns a default.  
        """  
        if not isinstance(input\_data, list) or not all(isinstance(i, list) for i in input\_data):  
            raise ValueError("Input data must be a list of lists of floats.")

        if self.is\_trained and self.model:  
            logger.debug(f"Making prediction using trained model for input: {input\_data}")  
            return self.model.predict(np.array(input\_data)).tolist()  
        else:  
            logger.warning("DummyClassifier not trained. Returning default prediction (0).")  
            \# Return a default prediction if not trained  
            return  \* len(input\_data)

\# Example usage (for testing within this module)  
if \_\_name\_\_ \== "\_\_main\_\_":  
    dummy\_model \= DummyClassifier()  
      
    \# Example training data (Iris dataset-like)  
    X\_train \= \[\[5.1, 3.5, 1.4, 0.2\], \[4.9, 3.0, 1.4, 0.2\], \[6.3, 3.3, 6.0, 2.5\]\]  
    y\_train \=   
    dummy\_model.train(X\_train, y\_train)

    \# Test prediction  
    test\_input \= \[\[5.0, 3.4, 1.5, 0.2\]\]  
    prediction \= dummy\_model.predict(test\_input)  
    print(f"Prediction for {test\_input}: {prediction}")

    test\_input\_untrained \= \[\[1.0, 1.0, 1.0, 1.0\]\]  
    untrained\_model \= DummyClassifier()  
    prediction\_untrained \= untrained\_model.predict(test\_input\_untrained)  
    print(f"Prediction for {test\_input\_untrained} (untrained model): {prediction\_untrained}")

### **my-mcp-server/src/my\_mcp\_server/models/model\_loader.py**

Python

\# src/my\_mcp\_server/models/model\_loader.py  
"""  
Handles the loading and management of machine learning models.  
This module can be extended to support dynamic model loading, versioning,  
and different model storage backends (e.g., S3, local disk).  
"""

import logging  
import os  
import pickle \# For simple model serialization \[15, 29\]  
from my\_mcp\_server.models.my\_llm\_model import DummyClassifier

logger \= logging.getLogger(\_\_name\_\_)

MODEL\_PATH \= os.path.join(os.path.dirname(\_\_file\_\_), "..", "..", "..", "data", "example\_model.pkl")

def load\_model\_from\_disk(path: str):  
    """  
    Loads a model from a specified disk path using pickle.  
    In a production environment, consider safer serialization methods (e.g., JSON, joblib, or model-specific formats)  
    and robust error handling. \[29, 30\]  
    """  
    if not os.path.exists(path):  
        logger.warning(f"Model file not found at {path}. Training a new dummy model.")  
        \# Create and train a dummy model if not found  
        dummy\_model \= DummyClassifier()  
        \# Provide some dummy training data for the classifier to be "trained"  
        X\_dummy \= \[\[5.1, 3.5, 1.4, 0.2\], \[4.9, 3.0, 1.4, 0.2\], \[6.3, 3.3, 6.0, 2.5\], \[5.8, 2.7, 5.1, 1.9\]\]  
        y\_dummy \=   
        dummy\_model.train(X\_dummy, y\_dummy)  
        save\_model\_to\_disk(dummy\_model, path)  
        return dummy\_model

    try:  
        with open(path, 'rb') as f:  
            model \= pickle.load(f)  
        logger.info(f"Model loaded successfully from {path}")  
        return model  
    except Exception as e:  
        logger.error(f"Failed to load model from {path}: {e}", exc\_info=True)  
        raise

def save\_model\_to\_disk(model, path: str):  
    """  
    Saves a model to a specified disk path using pickle.  
    Ensures the directory exists.  
    """  
    os.makedirs(os.path.dirname(path), exist\_ok=True)  
    try:  
        with open(path, 'wb') as f:  
            pickle.dump(model, f)  
        logger.info(f"Model saved successfully to {path}")  
    except Exception as e:  
        logger.error(f"Failed to save model to {path}: {e}", exc\_info=True)  
        raise

def get\_example\_model():  
    """  
    Retrieves the example ML model, loading it if necessary.  
    This function can be extended to implement dynamic model loading based on configuration  
    or runtime requirements. \[19, 31\]  
    """  
    logger.info("Attempting to get example model.")  
    return load\_model\_from\_disk(MODEL\_PATH)

if \_\_name\_\_ \== "\_\_main\_\_":  
    \# Example of how to use the model loader  
    model \= get\_example\_model()  
    print(f"Loaded model type: {type(model)}")

    \# Test prediction with the loaded model  
    test\_input \= \[\[5.0, 3.4, 1.5, 0.2\]\]  
    prediction \= model.predict(test\_input)  
    print(f"Prediction from loaded model for {test\_input}: {prediction}")

### **my-mcp-server/src/my\_mcp\_server/tools/\_\_init\_\_.py**

Python

\# src/my\_mcp\_server/tools/\_\_init\_\_.py  
"""  
This subpackage contains implementations of MCP-defined tools/capabilities  
that the server exposes to LLMs.  
"""

### **my-mcp-server/src/my\_mcp\_server/tools/filesystem\_tool.py**

Python

\# src/my\_mcp\_server/tools/filesystem\_tool.py  
"""  
Implements a simple filesystem tool for the MCP server.  
This tool can read content from a specified file.  
"""

import logging  
import os

logger \= logging.getLogger(\_\_name\_\_)

def read\_file\_content(file\_path: str) \-\> str:  
    """  
    Reads the content of a specified file.  
    This is a simplified example. In a real MCP server, this tool would  
    have robust security checks (e.g., path sanitization, access control)  
    to prevent unauthorized file access. \[9\]

    Args:  
        file\_path (str): The path to the file to read.

    Returns:  
        str: The content of the file.

    Raises:  
        FileNotFoundError: If the file does not exist.  
        PermissionError: If the server does not have permission to read the file.  
        IOError: For other I/O related errors.  
    """  
    logger.info(f"Attempting to read file: {file\_path}")  
    if not os.path.exists(file\_path):  
        logger.error(f"File not found: {file\_path}")  
        raise FileNotFoundError(f"File not found: {file\_path}")

    try:  
        with open(file\_path, 'r', encoding='utf-8') as f:  
            content \= f.read()  
        logger.info(f"Successfully read file: {file\_path}")  
        return content  
    except PermissionError:  
        logger.error(f"Permission denied to read file: {file\_path}")  
        raise PermissionError(f"Permission denied to read file: {file\_path}")  
    except Exception as e:  
        logger.error(f"Error reading file {file\_path}: {e}", exc\_info=True)  
        raise IOError(f"Error reading file {file\_path}: {e}")

\# Example usage (for testing within this module)  
if \_\_name\_\_ \== "\_\_main\_\_":  
    \# Create a dummy file for testing  
    dummy\_file\_path \= "test\_file.txt"  
    with open(dummy\_file\_path, "w") as f:  
        f.write("This is a test file for the filesystem tool.\\n")  
        f.write("It contains multiple lines of text.")

    try:  
        content \= read\_file\_content(dummy\_file\_path)  
        print(f"Content of '{dummy\_file\_path}':\\n{content}")  
    except Exception as e:  
        print(f"Error: {e}")  
    finally:  
        \# Clean up the dummy file  
        if os.path.exists(dummy\_file\_path):  
            os.remove(dummy\_file\_path)  
            print(f"Cleaned up {dummy\_file\_path}")

### **my-mcp-server/src/my\_mcp\_server/context/\_\_init\_\_.py**

Python

\# src/my\_mcp\_server/context/\_\_init\_\_.py  
"""  
This subpackage handles the dynamic management of contextual information  
for LLMs within the MCP server.  
"""

### **my-mcp-server/src/my\_mcp\_server/context/context\_manager.py**

Python

\# src/my\_mcp\_server/context/context\_manager.py  
"""  
Manages the dynamic context for the MCP server.  
This includes fetching, transforming, and persisting contextual information.  
"""

import logging  
import time  
from typing import Dict, Any  
\# from my\_mcp\_server.context.persistence import load\_context, save\_context \# Example for persistence

logger \= logging.getLogger(\_\_name\_\_)

\# In a real application, this would be a more sophisticated context store,  
\# potentially backed by a database or a vector store for LLM memory. \[22, 23, 31\]  
\_current\_context: Dict\[str, Any\] \= {  
    "session\_id": "initial\_session",  
    "user\_preferences": {"theme": "dark", "language": "en"},  
    "last\_activity\_timestamp": time.time(),  
    "active\_tools": \["read\_file", "search\_web"\],  
    "model\_capabilities": \["text\_generation", "summarization"\]  
}

def get\_current\_context() \-\> Dict\[str, Any\]:  
    """  
    Retrieves the current contextual information.  
    This function simulates fetching context. In a real scenario, it might  
    query a database, a cache, or an external context service. \[10, 32\]  
    """  
    logger.info("Fetching current context.")  
    \# Simulate dynamic context updates  
    \_current\_context\["last\_activity\_timestamp"\] \= time.time()  
    return \_current\_context.copy()

def update\_context(key: str, value: Any):  
    """  
    Updates a specific key in the current context.  
    This could trigger persistence mechanisms in a real application. \[10\]  
    """  
    logger.info(f"Updating context: {key} \= {value}")  
    \_current\_context\[key\] \= value  
    \# Example: save\_context(\_current\_context) \# If persistence is enabled

def add\_to\_context\_history(event: Dict\[str, Any\]):  
    """  
    Adds an event to a historical context log.  
    Useful for tracking interactions and building long-term memory. \[23\]  
    """  
    logger.info(f"Adding event to context history: {event}")  
    \# In a real system, this would write to a persistent log or a vector store.  
    if "history" not in \_current\_context:  
        \_current\_context\["history"\] \=  
    \_current\_context\["history"\].append(event)

\# Example usage (for testing within this module)  
if \_\_name\_\_ \== "\_\_main\_\_":  
    print("Initial context:", get\_current\_context())

    update\_context("user\_id", "user\_123")  
    print("Context after update:", get\_current\_context())

    add\_to\_context\_history({"action": "tool\_executed", "tool": "read\_file", "status": "success"})  
    print("Context with history:", get\_current\_context())

### **my-mcp-server/src/my\_mcp\_server/context/persistence.py**

Python

\# src/my\_mcp\_server/context/persistence.py  
"""  
Handles the persistence of contextual data.  
This module can be expanded to integrate with various storage solutions  
like databases (SQL/NoSQL), object storage (S3), or specialized vector stores. \[22, 23, 30\]  
"""

import json  
import os  
import logging  
from typing import Dict, Any

logger \= logging.getLogger(\_\_name\_\_)

\# Define a simple file path for context persistence (for demonstration)  
CONTEXT\_FILE\_PATH \= os.path.join(os.path.dirname(\_\_file\_\_), "..", "..", "..", "data", "context\_state.json")

def load\_context() \-\> Dict\[str, Any\]:  
    """  
    Loads context from a persistent storage (e.g., a JSON file).  
    """  
    if not os.path.exists(CONTEXT\_FILE\_PATH):  
        logger.info(f"Context file not found at {CONTEXT\_FILE\_PATH}. Returning empty context.")  
        return {}  
    try:  
        with open(CONTEXT\_FILE\_PATH, 'r', encoding='utf-8') as f:  
            context \= json.load(f)  
        logger.info(f"Context loaded successfully from {CONTEXT\_FILE\_PATH}")  
        return context  
    except json.JSONDecodeError as e:  
        logger.error(f"Error decoding JSON from context file {CONTEXT\_FILE\_PATH}: {e}", exc\_info=True)  
        return {}  
    except Exception as e:  
        logger.error(f"Failed to load context from {CONTEXT\_FILE\_PATH}: {e}", exc\_info=True)  
        return {}

def save\_context(context\_data: Dict\[str, Any\]):  
    """  
    Saves context to a persistent storage (e.g., a JSON file).  
    Ensures the directory exists before saving.  
    """  
    os.makedirs(os.path.dirname(CONTEXT\_FILE\_PATH), exist\_ok=True)  
    try:  
        with open(CONTEXT\_FILE\_PATH, 'w', encoding='utf-8') as f:  
            json.dump(context\_data, f, indent=4)  
        logger.info(f"Context saved successfully to {CONTEXT\_FILE\_PATH}")  
    except Exception as e:  
        logger.error(f"Failed to save context to {CONTEXT\_FILE\_PATH}: {e}", exc\_info=True)

\# Example usage (for testing within this module)  
if \_\_name\_\_ \== "\_\_main\_\_":  
    \# Test saving and loading  
    test\_context \= {  
        "user\_id": "test\_user\_1",  
        "preferences": {"notifications": True},  
        "history": \["login", "view\_dashboard"\]  
    }  
    save\_context(test\_context)

    loaded\_test\_context \= load\_context()  
    print("Loaded context:", loaded\_test\_context)

    \# Clean up the test file  
    if os.path.exists(CONTEXT\_FILE\_PATH):  
        os.remove(CONTEXT\_FILE\_PATH)  
        print(f"Cleaned up {CONTEXT\_FILE\_PATH}")

### **my-mcp-server/src/my\_mcp\_server/config/\_\_init\_\_.py**

Python

\# src/my\_mcp\_server/config/\_\_init\_\_.py  
"""  
This subpackage manages application configuration settings.  
"""

### **my-mcp-server/src/my\_mcp\_server/config/settings.py**

Python

\# src/my\_mcp\_server/config/settings.py  
"""  
Defines application settings using Pydantic-settings for robust configuration management.  
Settings can be loaded from environment variables and.env files. \[24, 25\]  
"""

import os  
from pydantic\_settings import BaseSettings, SettingsConfigDict  
from pydantic import Field  
import logging

class Settings(BaseSettings):  
    """  
    Application settings for the MCP Server.  
    Settings are loaded from environment variables and a.env file.  
    """  
    model\_config \= SettingsConfigDict(  
        env\_file='.env',  
        env\_file\_encoding='utf-8',  
        extra='ignore' \# Ignore extra environment variables not defined here  
    )

    APP\_NAME: str \= "MyMCPService"  
    APP\_VERSION: str \= "0.1.0"  
    DEBUG\_MODE: bool \= Field(False, description="Enable debug mode for development.")

    SERVER\_HOST: str \= Field("0.0.0.0", description="Host address for the server.")  
    SERVER\_PORT: int \= Field(8000, description="Port for the server to listen on.")

    LOG\_LEVEL: str \= Field("INFO", description="Logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL).")

    \# Example of a sensitive setting (should be in.env or secrets manager)  
    API\_KEY\_SECRET: str \= Field("supersecretkey", description="Secret key for API authentication.")

    \# Example of a model-specific setting  
    DEFAULT\_ML\_MODEL: str \= Field("dummy\_classifier", description="Name of the default ML model to use.")

    \# You can add more settings here as your application grows  
    \# For example, database connection strings, external service URLs, etc.

\# Instantiate settings to be imported across the application  
settings \= Settings()

\# Basic validation/logging of loaded settings  
if settings.DEBUG\_MODE:  
    logging.getLogger(\_\_name\_\_).setLevel(logging.DEBUG)  
    logging.getLogger(\_\_name\_\_).debug("Debug mode is enabled. Sensitive information might be logged.")  
    logging.getLogger(\_\_name\_\_).debug(f"Loaded settings: {settings.model\_dump\_json(indent=2)}")  
else:  
    logging.getLogger(\_\_name\_\_).setLevel(logging.INFO)  
    logging.getLogger(\_\_name\_\_).info("Production mode. Debugging is disabled.")

\# Ensure sensitive data is not accidentally logged in production  
if not settings.DEBUG\_MODE:  
    logging.getLogger(\_\_name\_\_).info("API\_KEY\_SECRET is loaded (masked for security).")  
    \# In a real app, you'd mask or avoid logging sensitive values directly \[5, 27\]

### **my-mcp-server/src/my\_mcp\_server/utils/\_\_init\_\_.py**

Python

\# src/my\_mcp\_server/utils/\_\_init\_\_.py  
"""  
This subpackage contains general utility functions that are reusable  
across different parts of the MCP server.  
"""

### **my-mcp-server/src/my\_mcp\_server/utils/helpers.py**

Python

\# src/my\_mcp\_server/utils/helpers.py  
"""  
Contains various helper functions for common tasks.  
"""

import uuid  
import logging

logger \= logging.getLogger(\_\_name\_\_)

def generate\_unique\_id() \-\> str:  
    """  
    Generates a universally unique identifier (UUID).  
    """  
    unique\_id \= str(uuid.uuid4())  
    logger.debug(f"Generated unique ID: {unique\_id}")  
    return unique\_id

def sanitize\_input\_string(input\_str: str) \-\> str:  
    """  
    A placeholder for input sanitization.  
    In a real application, this would involve more robust cleaning  
    to prevent injection attacks or unexpected data. \[15, 33\]  
    """  
    if not isinstance(input\_str, str):  
        logger.warning(f"Non-string input provided for sanitization: {type(input\_str)}")  
        return str(input\_str) \# Convert to string defensively

    sanitized\_str \= input\_str.strip()  
    \# Example: Remove potentially harmful characters or sequences  
    \# sanitized\_str \= sanitized\_str.replace("\<script\>", "")  
    logger.debug(f"Sanitized input: '{input\_str}' \-\> '{sanitized\_str}'")  
    return sanitized\_str

\# Example usage (for testing within this module)  
if \_\_name\_\_ \== "\_\_main\_\_":  
    print("Generated ID:", generate\_unique\_id())  
    print("Sanitized string:", sanitize\_input\_string("  Hello World\! \<script\>alert('xss')\</script\>  "))  
    print("Sanitized non-string:", sanitize\_input\_string(123))

### **my-mcp-server/tests/unit/test\_models.py**

Python

\# tests/unit/test\_models.py  
"""  
Unit tests for the models subpackage.  
"""

import pytest  
from my\_mcp\_server.models.my\_llm\_model import DummyClassifier  
from my\_mcp\_server.models.model\_loader import load\_model\_from\_disk, save\_model\_to\_disk  
import os

\# Define a temporary path for model persistence during tests  
TEST\_MODEL\_PATH \= "test\_example\_model.pkl"

@pytest.fixture(scope="module", autouse=True)  
def cleanup\_test\_model\_file():  
    """Fixture to ensure test model file is cleaned up before and after tests."""  
    if os.path.exists(TEST\_MODEL\_PATH):  
        os.remove(TEST\_MODEL\_PATH)  
    yield  
    if os.path.exists(TEST\_MODEL\_PATH):  
        os.remove(TEST\_MODEL\_PATH)

def test\_dummy\_classifier\_initialization():  
    """Test that DummyClassifier initializes correctly."""  
    model \= DummyClassifier()  
    assert not model.is\_trained  
    assert model.model is None

def test\_dummy\_classifier\_prediction\_untrained():  
    """Test prediction behavior when the model is not trained."""  
    model \= DummyClassifier()  
    test\_input \= \[\[1.0, 2.0, 3.0, 4.0\]\]  
    prediction \= model.predict(test\_input)  
    assert prediction \==  \# Expect default prediction when untrained

def test\_dummy\_classifier\_training\_and\_prediction():  
    """Test that DummyClassifier can be trained and makes a prediction."""  
    model \= DummyClassifier()  
    X\_train \= \[\[5.1, 3.5, 1.4, 0.2\], \[4.9, 3.0, 1.4, 0.2\], \[6.3, 3.3, 6.0, 2.5\]\]  
    y\_train \=   
    model.train(X\_train, y\_train)  
    assert model.is\_trained  
    assert model.model is not None

    test\_input \= \[\[5.0, 3.4, 1.5, 0.2\]\]  
    prediction \= model.predict(test\_input)  
    assert isinstance(prediction, list)  
    assert len(prediction) \== 1  
    assert prediction in  \# Based on RandomForestClassifier output

def test\_model\_loader\_save\_and\_load():  
    """Test that models can be saved to and loaded from disk."""  
    original\_model \= DummyClassifier()  
    X\_train \= \[\[5.1, 3.5, 1.4, 0.2\], \[4.9, 3.0, 1.4, 0.2\], \[6.3, 3.3, 6.0, 2.5\]\]  
    y\_train \=   
    original\_model.train(X\_train, y\_train)

    save\_model\_to\_disk(original\_model, TEST\_MODEL\_PATH)  
    assert os.path.exists(TEST\_MODEL\_PATH)

    loaded\_model \= load\_model\_from\_disk(TEST\_MODEL\_PATH)  
    assert isinstance(loaded\_model, DummyClassifier)  
    assert loaded\_model.is\_trained

    \# Verify predictions are consistent after loading  
    test\_input \= \[\[5.0, 3.4, 1.5, 0.2\]\]  
    original\_prediction \= original\_model.predict(test\_input)  
    loaded\_prediction \= loaded\_model.predict(test\_input)  
    assert original\_prediction \== loaded\_prediction

def test\_model\_loader\_non\_existent\_file\_creates\_new\_model():  
    """Test that loading a non-existent file creates and trains a new model."""  
    if os.path.exists(TEST\_MODEL\_PATH):  
        os.remove(TEST\_MODEL\_PATH) \# Ensure it doesn't exist

    new\_model \= load\_model\_from\_disk(TEST\_MODEL\_PATH)  
    assert isinstance(new\_model, DummyClassifier)  
    assert new\_model.is\_trained \# Should be trained with dummy data  
    assert os.path.exists(TEST\_MODEL\_PATH) \# Should have created the file

### **my-mcp-server/configs/development.yaml**

YAML

\# configs/development.yaml  
\# Development-specific configuration settings.  
\# These settings can be overridden by environment variables or.env file.

app\_name: "MyMCPService-Dev"  
debug\_mode: true

server\_host: "127.0.0.1"  
server\_port: 8000

log\_level: "DEBUG"

\# Example of a development-specific database connection (not for production\!)  
database:  
  type: "sqlite"  
  path: "./data/dev\_db.sqlite"

\# Example of an external service URL for development  
external\_api:  
  url: "http://dev.external-api.com/v1"  
  timeout\_seconds: 5

I've added these sample files to the project structure. This template provides a solid starting point for building your MCP Server, incorporating best practices for clarity, maintainability, and AI/LLM compatibility.

Let me know if you'd like any further modifications or elaborations on specific parts\!No problem. I'm working on your updated research. Here's the project structure with sample Python and other files to create a template for your Model Context Protocol (MCP) Server.

# **Standard Python Project Structure for a Model Context Protocol (MCP) Server**

## **Executive Summary**

This report delineates a comprehensive, standard project structure for a Python-based Model Context Protocol (MCP) Server. The proposed architecture prioritizes modularity, clear interfaces, and machine-readable metadata, ensuring optimal understanding and usability by human developers, AI agents, and Large Language Models (LLMs). Key recommendations include adopting the src layout for packaging clarity, implementing robust documentation and code quality standards, and integrating specialized modules for AI/ML models, MCP-specific tools, and advanced context management. By adhering to these principles, the MCP Server will be robust, maintainable, scalable, secure, and inherently more interpretable by intelligent systems.

A fundamental consideration throughout this architectural design is the requirement for the project structure to be comprehensible by humans, AI, and LLMs. While traditional software engineering emphasizes human readability, the need for AI/LLM understanding necessitates a focus on machine-parseable, structured information. Elements such as clear docstrings 1, explicit type hints 2, consistent naming conventions 1, and rigorously structured configuration files 4 are not merely beneficial practices for human developers; they constitute critical data points for AI and LLM agents that attempt to interpret or interact with the codebase. The

src layout, for instance, assists AI/LLMs by distinctly separating importable code from other project artifacts.7 This implies that standard software engineering best practices, when applied with precision and an awareness of machine-readability, inherently fulfill the requirement for AI/LLM comprehension. The design does not involve writing code

*for* AI, but rather writing *well-structured, well-documented* code that AI can then more readily parse and interpret. The subsequent sections detail how each structural and coding practice contributes to both human and machine comprehension, treating them as interconnected aspects of a single objective.

## **Introduction to Model Context Protocol (MCP) Servers**

### **Defining MCP: Bridging LLMs with External Tools and Data**

The Model Context Protocol (MCP), introduced by Anthropic in late 2024, represents a significant advancement in the integration of AI systems. It is an open standard specifically designed to bridge the gap between AI assistants, particularly Large Language Models (LLMs), and the diverse, data-rich ecosystems they need to navigate.8 This protocol provides a universal framework that connects LLMs with external tools and data sources, granting AI systems seamless access to relevant contexts.8 By offering a standardized approach for LLM applications to share contextual information, expose specialized tools and capabilities, and construct composable integrations, MCP effectively eliminates the fragmented and often inconsistent integrations that have historically challenged AI development.8

### **The Core Architecture: Hosts, Clients, Servers, and Their Interactions**

MCP operates on a secure and efficient client-host-server paradigm, built upon JSON-RPC 2.0 messages, which facilitates stateful sessions for the exchange of context and sampling operations.8

* **Hosts:** These entities are typically the LLM applications themselves, functioning as containers or coordinators for multiple client instances. Hosts are responsible for managing the lifecycle of these instances and enforcing crucial security policies, including permissions, user authorization, and consent requirements.8  
* **Clients:** Each client operates within a host and is tasked with capability negotiation and the orchestration of messages between itself and an MCP server. Clients are designed to maintain strict security boundaries, ensuring that resources belonging to one client cannot be accessed by another.8  
* **Servers:** These are the services that furnish context and capabilities to the AI systems.9 MCP Servers encapsulate external data sources, APIs, or other utilities, such as Customer Relationship Management (CRM) systems, Git repositories, or file systems. They define and expose three primary types of functionalities: "Resources" (contextual data for user or AI model consumption), "Prompts" (templated messages for users), and "Tools" (functions that the AI model can execute).8 Servers are mandated to adhere rigorously to the security constraints and user permissions enforced by the host.8

### **Why a Well-Structured Python Server is Critical for MCP Adoption**

The development of a well-structured Python server for MCP is of paramount importance. Such a structure ensures the reproducibility of AI environments, promotes standardization and collaboration across development teams, and facilitates secure, efficient access to diverse data and tools.8 This architectural rigor is indispensable for production-grade AI systems, as it directly contributes to maintainability, scalability, and testability.2 The Model Context Protocol Python SDK further streamlines the development process by offering advanced context management and intelligent routing capabilities.10

MCP servers are often conceptualized as the "AI ecosystem's toolboxes".8 This analogy underscores a critical architectural implication: by exposing consistent interfaces, these servers resolve the "N times M problem," where distinct integrations would otherwise be required for every AI application and every data source.8 A server, functioning as a toolbox, must possess a highly organized internal structure, with clearly labeled tools (modules and functions), easily accessible interfaces (APIs), and robust mechanisms to guarantee tool reliability (e.g., comprehensive testing and error handling). This directly translates into an architectural imperative for clear modularization 1, consistent naming 1, and well-defined API endpoints. The resolution of the "N times M problem" also implies that the server must be sufficiently generic to serve a variety of clients, thereby reinforcing the necessity for a standardized and extensible structure. Modular design, in this context, transcends mere coding preference; it becomes an architectural necessity for effective AI/LLM interaction within the MCP framework. The project structure must explicitly cater to this "toolbox" concept by incorporating dedicated, well-defined sections for

tools/ and models/, supported by a robust API layer that serves as the primary access point to these capabilities.

**Table 1: Key MCP Server Components and Their Purpose**

| Component | Purpose/Role within MCP | Relevance to Python Server |
| :---- | :---- | :---- |
| **Host** | LLM application; coordinates clients, manages lifecycle, enforces security (permissions, consent). | The Python server acts *as* a Server for a Host. Its design must anticipate and integrate with Host-enforced security and communication protocols. |
| **Client** | Connects within Host; negotiates capabilities, orchestrates messages with Server, maintains security boundaries. | The Python server *receives* requests from Clients. Its API design must align with Client expectations for tool invocation, resource requests, and prompt handling. |
| **Server** | Provides context and capabilities to AI systems; wraps data sources, APIs, utilities. | The Python server *is* the Server. Its internal structure directly reflects the organization and exposure of its Resources, Prompts, and Tools. |
| **Resources** | Contextual information and data for user or AI model use. | The Python server must implement modules to fetch, process, and serialize data as Resources, ensuring they are accessible and relevant to LLMs. |
| **Prompts** | Templated messages and workflows for users. | The Python server may host or generate dynamic Prompts, requiring structured templates and logic within its codebase. |
| **Tools** | Functions for the AI model to execute. | The Python server's core functionality often involves exposing Python functions as Tools, necessitating clear interfaces, input validation, and robust execution logic. |
| **Sampling** | Server-initiated agentic behaviors and recursive LLM interactions (client may offer this to server). | While primarily a client feature, the Python server's architecture should be capable of handling and responding to sampling requests if it offers agentic capabilities. |

## **Foundational Python Project Structure for Clarity**

### **Core Principles: Modularity, Separation of Concerns, High Cohesion, Low Coupling**

The bedrock of any robust and comprehensible Python project, including an MCP server, lies in adherence to core software engineering principles: modularity, separation of concerns, high cohesion, and low coupling. These principles are fundamental for creating applications that are not only maintainable and scalable but also inherently reusable and testable.1

**Modularity** involves the decomposition of large, complex programming tasks into smaller, independent, and reusable subtasks or modules.1 This approach simplifies the development process by enabling developers to focus on individual components in isolation. It significantly enhances maintainability, as changes or bug fixes can be localized to specific modules without affecting the entire system. Furthermore, modularity promotes code reusability across different parts of the project or even in future projects, reduces duplication of code, and helps prevent namespace conflicts by allowing each module to define its own distinct namespace.1

**Separation of Concerns** dictates that each module or component within the project should possess a single, well-defined responsibility. This prevents the intertwining of functionalities, which can lead to complex dependencies and make the codebase difficult to manage and understand.2 When a module is responsible for only one aspect of the system, its purpose becomes immediately clear to both human developers and automated analysis tools.

**High Cohesion** ensures that related functionality is logically grouped together within a single module.2 This principle fosters a natural organization where all elements necessary for a specific task reside in one place, making the module self-contained and easier to comprehend.

Conversely, **Low Coupling** minimizes the dependencies between modules.2 This means that modifications within one module should have minimal, if any, impact on other modules. Such independence is crucial for large-scale projects and collaborative development environments, as it reduces the likelihood of introducing unintended side effects and simplifies the process of integrating changes from multiple team members.1

The application of these principles naturally creates well-defined boundaries for functionalities. An MCP server, by design, exposes "Tools" and "Resources" to LLMs.8 For an LLM to effectively utilize these capabilities, it must comprehend their precise boundaries, expected inputs, and anticipated outputs. When a "tool" is implemented as a cohesive, low-coupling module, its function is isolated and predictable. This clarity allows the LLM to more reliably invoke the tool and interpret its results. In contrast, a monolithic or highly coupled codebase obscures the identification and utilization of specific functionalities as "tools" due to interwoven logic and ambiguous responsibilities. Therefore, modular design is not merely a coding preference; it is an architectural imperative for effective AI/LLM interaction within the MCP framework, as it directly translates to the clarity and reliability of exposed capabilities.

### **Recommended Directory Layout**

A consistent and logical directory layout is paramount for project navigability, maintainability, and scalability. The following structure is recommended for an MCP Server, incorporating best practices for human comprehension, efficient development workflows, and machine-readability for AI/LLM systems.

my-mcp-server/  
├── src/  
│   └── my\_mcp\_server/  
│       ├── \_\_init\_\_.py  
│       ├── \_\_main\_\_.py  
│       ├── server.py  
│       ├── models/  
│       │   ├── \_\_init\_\_.py  
│       │   ├── my\_llm\_model.py  
│       │   └── model\_loader.py  
│       ├── tools/  
│       │   ├── \_\_init\_\_.py  
│       │   ├── filesystem\_tool.py  
│       │   ├── database\_tool.py  
│       │   └── web\_fetch\_tool.py  
│       ├── context/  
│       │   ├── \_\_init\_\_.py  
│       │   ├── context\_manager.py  
│       │   └── persistence.py  
│       ├── config/  
│       │   ├── \_\_init\_\_.py  
│       │   └── settings.py  
│       └── utils/  
│           ├── \_\_init\_\_.py  
│           └── helpers.py  
├── tests/  
│   ├── unit/  
│   │   ├── test\_models.py  
│   │   └── test\_tools.py  
│   ├── integration/  
│   │   └── test\_server\_integration.py  
│   └── performance/  
│       └── test\_load.py  
├── docs/  
│   ├── README.md  
│   ├── api\_spec.json  (Auto-generated OpenAPI/Swagger)  
│   └── usage\_guide.md  
├── data/ (Optional: Sample data, pre-trained model artifacts)  
│   ├── sample\_data.json  
│   └── pre\_trained\_model.bin  
├── configs/ (Environment-specific configuration files)  
│   ├── development.yaml  
│   ├── production.yaml  
│   └── testing.yaml  
├── venv/ (Virtual environment directory \- typically.gitignore'd)  
├──.gitignore  
├── pyproject.toml  
└── README.md  
└── LICENSE

#### **my-mcp-server/ (Root Project Directory)**

This is the top-level container for the entire project, establishing a clear boundary for the codebase. Its primary function is to serve as the entry point for developers and automated systems alike, providing a high-level overview of the project's contents.

#### **src/ (Source Code \- leveraging src layout for packaging and import clarity)**

The src layout is highly recommended for Python projects, particularly those intended for distribution as packages or deployed in complex environments. This structure places all importable Python code into a dedicated src/ subdirectory.7

The adoption of the src layout offers several advantages. It effectively prevents the accidental usage of in-development code by ensuring that the installed copy of the package is utilized during runtime, rather than potentially conflicting local files.7 This is a critical safeguard, as the Python interpreter includes the current working directory as the first item on its import path. Without the

src layout, an import package in the current working directory with the same name as an installed package would be used, potentially leading to subtle misconfigurations and issues where files are inadvertently excluded from a distribution.7 Furthermore, this layout enforces that editable installations (e.g.,

pip install \-e.) only import files explicitly intended to be part of the package. This prevents non-code project files, such as README.md or tox.ini, from being inadvertently added to the Python import path, which could cause imports to function in development but fail in production.7 Such consistency is invaluable for reliable behavior across development, testing, and production environments. The

create-mcp-server tool, a reference implementation for MCP projects, already employs this structure 11, reinforcing its status as a best practice in this domain.

A minor consideration of the src layout is that running command-line interfaces directly from the source tree typically necessitates the project's installation, often in editable mode.7 However, common workarounds, such as invoking the package via

python \-m my\_mcp\_server, are often preferred for production execution and are well-supported.12

The src layout provides a distinct advantage for AI/LLM consumption by establishing a clear "contract" regarding what constitutes the "public API" of the Python package. The primary benefit of this layout—preventing accidental imports and ensuring consistent behavior between development and deployed environments 7—translates directly into a more reliable understanding for an AI or LLM. If an LLM is tasked with generating code or interacting with the server, knowing that only code within

src/ is genuinely importable and stable helps it avoid generating invalid or brittle interactions. This structure provides an unambiguous boundary for what the "server" represents from an importable code perspective, thereby reducing ambiguity in an AI's comprehension of the codebase's exposed functionalities. Documenting the use of the src layout clearly within the README.md and potentially in machine-readable metadata (e.g., pyproject.toml comments or a separate schema) would further enhance AI/LLM understanding of the project's internal structure and import rules, making the project more "AI-friendly."

#### **my\_mcp\_server/ (Python Package for the Server)**

This subdirectory represents the actual Python package containing the server's core logic. It is essential for this directory to contain an \_\_init\_\_.py file to be recognized as an importable package by Python.4

* **\_\_init\_\_.py**: This special file serves as a signal to Python that the directory it resides in is a package.4 It can be an empty file or contain package-level initialization code, such as setting up global logging configurations or importing key submodules to facilitate easier access from outside the package.  
* **\_\_main\_\_.py**: This file provides a command-line entry point for the package, allowing it to be executed directly using the python \-m my\_mcp\_server command.11 Its design should be minimal, primarily importing and executing the main server application logic contained in  
  server.py.12  
* **server.py**: This file encapsulates the main server application logic. It typically integrates with a web framework like FastAPI to define and manage API endpoints.14 This file acts as the central orchestrator, responsible for handling incoming MCP requests, validating them, and dispatching them to the appropriate internal modules for processing.  
* **models/ (Subpackage for ML models and their loading/inference logic)**: This dedicated subpackage is designed to house all machine learning models (e.g., PyTorch, TensorFlow, Scikit-learn) that the MCP server might expose as "Tools" or utilize internally for its operations.14 This separation is crucial for effective management of model versions, handling model persistence, and implementing dynamic loading strategies. The directory should contain submodules for different model types or specific models, along with their respective loading and inference interfaces.  
  An MCP server's primary function is to provide context and tools to LLMs 8, which frequently involves serving ML models for inference.16 By placing models within a dedicated  
  models/ subpackage, the structure explicitly communicates to both human developers and AI/LLM agents that this is the repository of the system's "intelligence." The capability to dynamically load models 19 based on runtime context or specific LLM requests (e.g., an LLM requesting a particular version of a model) is a key feature for building scalable and flexible MCP servers. This modularity facilitates efficient model versioning 20, enables A/B testing of different model implementations 20, and supports optimized resource allocation.18 These capabilities allow the server to adapt seamlessly to varying demands and model updates without requiring service downtime. Consequently, the  
  models/ directory should contain not only the model artifacts themselves but also the code responsible for loading them (e.g., model\_loader.py) and defining their inference interfaces. This explicit structure enhances an LLM's ability to identify and interact with specific model capabilities, improving its overall utility.  
* **tools/ (Subpackage for MCP-defined tools/capabilities)**: This subpackage contains the Python implementations of the "Tools" that the MCP server exposes to LLMs.8 Each tool should ideally be implemented as a separate module or class within this directory, strictly adhering to the Single Responsibility Principle.2 This structural choice ensures that each tool's functionality is isolated and clearly defined.  
  LLMs are designed to "execute" tools exposed by an MCP server.9 For this tool-use to be effective and reliable, the tools must possess well-defined interfaces—including clear inputs, outputs, and predictable side effects—that an LLM can readily understand and interact with. Placing these implementations in a dedicated  
  tools/ directory, complemented by clear docstrings and type hints, directly supports this requirement. The directory structure itself, combined with consistent coding standards, becomes an integral part of the "API" for the LLM, enabling it to infer available functionalities and their proper invocation. This organization aligns perfectly with the "toolbox" analogy for MCP servers 8, where each tool is distinct and easily identifiable. Thus, each file or module within  
  tools/ should correspond to a distinct MCP tool, with its public functions clearly annotated and described for LLM consumption. This explicit organization and internal documentation are paramount for effective AI-driven tool utilization.  
* **context/ (Subpackage for MCP context management logic)**: This critical subpackage is responsible for the dynamic creation, multi-layered tracking, serialization, and intelligent routing of contextual information for LLMs.10 It may encompass logic for fetching context from various external sources, transforming raw data into a usable format, maintaining context state across multiple interactions, and managing caching mechanisms.  
  The core value proposition of MCP is its ability to provide "context" to LLMs, which is essential for them to generate relevant and informed responses.9 This context is not static; it is dynamic, often multi-layered, and requires active management.10 The presence of a  
  context/ subpackage implies a dedicated pipeline for context processing. This could involve complex data retrieval, sophisticated data transformation, efficient caching strategies 10, and even integration with long-term memory solutions such as vector stores.22 The concept of "intelligent routing" 10 suggests that this module must be adept at determining  
  *how* context is delivered to different models or parts of the system, optimizing for relevance and efficiency. Consequently, this module will likely contain intricate logic, potentially involving asynchronous operations 10 and external data persistence mechanisms, to ensure that context is consistently fresh, relevant, and efficiently delivered to the LLM. Its design must prioritize performance and reliability to prevent it from becoming a bottleneck for LLM interaction.  
* **config/ (Application configuration management)**: This subdirectory is exclusively dedicated to managing application settings, parameters, and environment-specific configurations (e.g., development, testing, production).4 It should leverage structured formats such as YAML, JSON, or INI for both human readability and machine parsing.4 Furthermore, it is imperative to integrate with environment variables for handling sensitive data like API keys, ensuring that such credentials are not hardcoded into the codebase.3  
  Configuration files abstract operational details from the core code, enhance security, and enable straightforward updates without modifying source code.5 For AI/LLMs, well-structured configuration files (e.g., YAML, JSON) function as a machine-readable blueprint of the server's operational parameters. An LLM could potentially parse these files to understand, for instance, which external services the server connects to, or what configurable behaviors are available (though sensitive values themselves would be securely managed outside the repository). This capability is crucial for automated deployment, troubleshooting, and even self-correction by advanced AI agents. The use of libraries like Pydantic for defining settings classes 24 further enforces type safety, which is highly valuable for machine parsing and validation. Therefore, the  
  config/ directory should contain clear, human-readable, and machine-parseable configuration files, potentially with schema definitions or examples, to facilitate automated understanding and management by AI/LLM systems.  
* **utils/ (General utility functions)**: This subpackage contains reusable helper functions that do not logically fit into other specific domains. Examples include common data transformations, general validation routines, or centralized logging setup utilities. Segregating these utilities prevents code duplication across the project and significantly improves overall maintainability.

#### **Top-Level Files and Directories**

* **tests/ (Comprehensive unit, integration, and functional tests)**: This top-level directory is fundamental for automated quality assurance, enabling the early detection of bugs and ensuring that the code functions as expected under various conditions.1 It should contain a well-organized suite of unit tests (for individual functions and modules), integration tests (for interactions between components), and performance tests (to assess scalability under load).25 Pytest is a widely recommended and powerful testing framework for Python projects.2  
  Tests define the expected behavior of the code.1 For human developers, they serve as executable examples, demonstrating how to use the code and what outputs to anticipate from specific functions or modules. For AI/LLMs, tests can be parsed as executable specifications. An LLM could potentially analyze test cases to infer the precise functionality, identify edge cases, and understand the expected outputs of a module or tool, especially when docstrings are insufficient or ambiguous. This process represents a powerful form of "learning by example" for the AI, enabling it to better comprehend the system's operational logic. Consequently, well-written, comprehensive tests with clear assertions are not merely for quality assurance; they constitute a valuable form of machine-readable documentation that can significantly enhance AI/LLM understanding and interaction with the codebase.  
* **docs/ (Project documentation, API specifications, usage guides)**: This directory serves as the centralized repository for all project documentation.1 It should include the  
  README.md (as detailed below), detailed API documentation (potentially auto-generated by tools like FastAPI's Swagger UI 14), design documents, and comprehensive usage guides for both developers and end-users.  
  While code structure and tests provide implicit signals, explicit documentation remains the most direct and comprehensive method to communicate intent, functionality, and usage to both human developers and AI/LLM systems. For LLMs, well-structured markdown (README.md), clear API specifications (e.g., OpenAPI/Swagger JSON), and detailed explanations of tools and context management are crucial for effective interaction. This is where the human-readable and AI-readable aspects of the project converge most strongly. Tools like FastAPI's auto-generated Swagger UI 14 exemplify documentation that is inherently machine-readable (via JSON schema) and simultaneously human-friendly, making it ideal for AI consumption. Therefore, significant investment in clear, consistent, and structured documentation is warranted. Consideration should be given to tools that generate machine-readable formats (e.g., OpenAPI/Swagger for APIs) to maximize AI/LLM comprehension and enable automated interactions.  
* **data/ (Optional: Sample data, pre-trained model artifacts)**: This is an optional directory for storing sample datasets, pre-trained model artifacts, or other static data assets required by the application for development or testing purposes.4 Its inclusion helps maintain a clean and focused main source code directory.  
* **configs/ (Environment-specific configuration files)**: This dedicated directory houses various environment-specific configuration files (e.g., development.yaml, production.yaml, testing.yaml).4 This separation allows for straightforward management of different settings across various deployment stages without requiring modifications to the core application code.  
* **venv/ (Virtual environment directory \- typically .gitignored)**: This directory contains the isolated Python virtual environment for the project.1 It should always be explicitly excluded from version control using a  
  .gitignore file to prevent the accidental commitment of large, environment-specific files.3  
* **.gitignore**: This file specifies intentionally untracked files that Git should ignore. It is crucial for preventing sensitive information (e.g., .env files 3) and large, auto-generated files (e.g., virtual environments 3) from being committed to the repository.  
* **pyproject.toml (Modern project metadata, dependencies, build system)**: This file represents the modern standard for Python project configuration, centralizing project metadata, dependencies, and build system definitions. It effectively supersedes older setup.py files.1 Tools such as Poetry or  
  uv manage dependencies and project creation based on the specifications within this file.2  
  The pyproject.toml file centralizes critical project metadata, dependencies, and build system configuration.1 For an AI/LLM, this file serves as the primary manifest of the project. It explicitly communicates to the AI the project's name, version, required dependencies, and instructions for building or installing it. This information is indispensable for automated setup, efficient dependency resolution, and a comprehensive understanding of the project's ecosystem, thereby enabling AI-driven development tools to interact seamlessly. Therefore, meticulous maintenance of  
  pyproject.toml to include all relevant project metadata is crucial. Its structured format makes it highly valuable for automated parsing and interpretation by AI/LLM systems.  
* **README.md (Project overview, setup, quick start, philosophy)**: This file is the primary entry point for any entity, human or AI, encountering the project repository.1 It should clearly articulate the project's purpose, provide concise installation and usage instructions, and outline the project's underlying philosophy.11 For an MCP server, it is particularly important to highlight its role within the broader AI ecosystem.  
  The README.md is often the first file a human or AI/LLM processes when exploring a new repository.1 Its clarity and completeness directly influence how quickly and accurately an AI/LLM can "onboard" to the project. A well-structured  
  README with clear headings, illustrative code examples, and explicit instructions (such as the create-mcp-server philosophy emphasizing "Zero Configuration," "Best Practices," and "Batteries Included" 11) provides a high-level semantic understanding that guides further exploration and interaction. It functions as a concise summary for initial AI comprehension. Consequently, the  
  README.md should be treated as a critical piece of user (and AI) documentation, not merely an afterthought. Its quality directly correlates with the ease of adoption and integration for both human developers and automated systems.  
* **LICENSE (Software license)**: This legal document defines the terms and conditions under which the software project can be used, modified, and distributed.4 Its inclusion is essential for any open-source project, clarifying intellectual property rights and usage permissions.

## **Sample Files for the Project Template**

Here are sample contents for the key files and directories to help you get started with your MCP Server template.

### **my-mcp-server/pyproject.toml**

Ini, TOML

\[project\]  
name \= "my-mcp-server"  
version \= "0.1.0"  
description \= "A Model Context Protocol (MCP) server for AI/LLM integration."  
authors \= \[{ name \= "Your Name", email \= "your.email@example.com" }\]  
dependencies \= \[  
    "fastapi\>=0.111.0",  
    "uvicorn\[standard\]\>=0.30.1",  
    "pydantic-settings\>=2.3.4",  
    "python-dotenv\>=1.0.0",  
    "mcp\[cli\]\>=1.4.0", \# Core MCP SDK dependency \[26\]  
    "scikit-learn\>=1.5.0", \# Example ML library \[14\]  
    "pandas\>=2.2.2", \# Example data handling library \[14\]  
requires-python \= "\>=3.10"  
readme \= "README.md"  
license \= { text \= "MIT" }

\[project.scripts\]  
my-mcp-server \= "my\_mcp\_server.\_\_main\_\_:main" \# Defines CLI entry point \[12\]

\[build-system\]  
requires \= \["poetry-core\>=1.0.0"\] \# Or "hatchling" if using Hatch  
build-backend \= "poetry.core.masonry.api" \# Or "hatchling.build"

### **my-mcp-server/README.md**

Markdown

\# My MCP Server

This repository contains a template for building a Model Context Protocol (MCP) server using Python. The server is designed to provide contextual information and expose tools to Large Language Models (LLMs) in a standardized, secure, and scalable manner.

\#\# Project Structure

The project follows a \`src\` layout for

#### **Works cited**

1. How to Structure Python Projects \- Dagster, accessed June 17, 2025, [https://dagster.io/blog/python-project-best-practices](https://dagster.io/blog/python-project-best-practices)  
2. How to design modular Python projects \- LabEx, accessed June 17, 2025, [https://labex.io/tutorials/python-how-to-design-modular-python-projects-420186](https://labex.io/tutorials/python-how-to-design-modular-python-projects-420186)  
3. 12 Steps to Organize and Maintain Your Python Codebase for Beginners \- DEV Community, accessed June 17, 2025, [https://dev.to/olgabraginskaya/12-steps-to-organize-and-maintain-your-python-codebase-for-beginners-18bb](https://dev.to/olgabraginskaya/12-steps-to-organize-and-maintain-your-python-codebase-for-beginners-18bb)  
4. The ultimate guide to structuring a Python package \- Retail Technology Innovation Hub, accessed June 17, 2025, [https://retailtechinnovationhub.com/home/2024/2/29/the-ultimate-guide-to-structuring-a-python-package](https://retailtechinnovationhub.com/home/2024/2/29/the-ultimate-guide-to-structuring-a-python-package)  
5. Working with Python Configuration Files: Tutorial & Best Practices \- Configu, accessed June 17, 2025, [https://configu.com/blog/working-with-python-configuration-files-tutorial-best-practices/](https://configu.com/blog/working-with-python-configuration-files-tutorial-best-practices/)  
6. Implementing Structured Logging in Python: A Comprehensive Guide | Graph AI, accessed June 17, 2025, [https://www.graphapp.ai/blog/implementing-structured-logging-in-python-a-comprehensive-guide](https://www.graphapp.ai/blog/implementing-structured-logging-in-python-a-comprehensive-guide)  
7. src layout vs flat layout \- Python Packaging User Guide, accessed June 17, 2025, [https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)  
8. A beginners Guide on Model Context Protocol (MCP) \- OpenCV, accessed June 17, 2025, [https://opencv.org/blog/model-context-protocol/](https://opencv.org/blog/model-context-protocol/)  
9. Specification \- Model Context Protocol, accessed June 17, 2025, [https://modelcontextprotocol.io/specification/2025-03-26](https://modelcontextprotocol.io/specification/2025-03-26)  
10. MCP Python Implementation: Build AI Tools with Model Context ..., accessed June 17, 2025, [https://www.byteplus.com/en/topic/541239](https://www.byteplus.com/en/topic/541239)  
11. modelcontextprotocol/create-python-server: Create a ... \- GitHub, accessed June 17, 2025, [https://github.com/modelcontextprotocol/create-python-server](https://github.com/modelcontextprotocol/create-python-server)  
12. Where can you learn how to set out project structure correctly? : r/learnpython \- Reddit, accessed June 17, 2025, [https://www.reddit.com/r/learnpython/comments/1k1klhy/where\_can\_you\_learn\_how\_to\_set\_out\_project/](https://www.reddit.com/r/learnpython/comments/1k1klhy/where_can_you_learn_how_to_set_out_project/)  
13. accessed December 31, 1969, [https://github.com/modelcontextprotocol/create-python-server/tree/main/src/my\_server](https://github.com/modelcontextprotocol/create-python-server/tree/main/src/my_server)  
14. How to Use FastAPI for Machine Learning | The PyCharm Blog, accessed June 17, 2025, [https://blog.jetbrains.com/pycharm/2024/09/how-to-use-fastapi-for-machine-learning/](https://blog.jetbrains.com/pycharm/2024/09/how-to-use-fastapi-for-machine-learning/)  
15. Creating a Secure Machine Learning API with FastAPI and Docker, accessed June 17, 2025, [https://machinelearningmastery.com/creating-a-secure-machine-learning-api-with-fastapi-and-docker/](https://machinelearningmastery.com/creating-a-secure-machine-learning-api-with-fastapi-and-docker/)  
16. Deploy models using Mosaic AI Model Serving \- Databricks Documentation, accessed June 17, 2025, [https://docs.databricks.com/aws/en/machine-learning/model-serving/](https://docs.databricks.com/aws/en/machine-learning/model-serving/)  
17. Best Tools For ML Model Serving \- Neptune.ai, accessed June 17, 2025, [https://neptune.ai/blog/ml-model-serving-best-tools](https://neptune.ai/blog/ml-model-serving-best-tools)  
18. Ray Serve: Scalable and Programmable Serving — Ray 2.47.0 \- Ray Docs, accessed June 17, 2025, [https://docs.ray.io/en/latest/serve/index.html](https://docs.ray.io/en/latest/serve/index.html)  
19. How to dynamically load a Python class \- Stack Overflow, accessed June 17, 2025, [https://stackoverflow.com/questions/547829/how-to-dynamically-load-a-python-class](https://stackoverflow.com/questions/547829/how-to-dynamically-load-a-python-class)  
20. BentoML: Simplifying AI Model Deployment \- BytePlus, accessed June 17, 2025, [https://www.byteplus.com/en/topic/536412](https://www.byteplus.com/en/topic/536412)  
21. Understanding Model Context: A Developer's Guide to AI and LLMs \- VideoSDK, accessed June 17, 2025, [https://www.videosdk.live/developer-hub/ai/model-context](https://www.videosdk.live/developer-hub/ai/model-context)  
22. Persisting & Loading Data \- LlamaIndex, accessed June 17, 2025, [https://docs.llamaindex.ai/en/stable/module\_guides/storing/save\_load/](https://docs.llamaindex.ai/en/stable/module_guides/storing/save_load/)  
23. A Long-Term Memory Agent | 🦜️ LangChain, accessed June 17, 2025, [https://python.langchain.com/docs/versions/migrating\_memory/long\_term\_memory\_agent/](https://python.langchain.com/docs/versions/migrating_memory/long_term_memory_agent/)  
24. Order in Chaos: Python Configuration Management for Enterprise Applications \- DZone, accessed June 17, 2025, [https://dzone.com/articles/order-in-chaos-python-configuration-management-for](https://dzone.com/articles/order-in-chaos-python-configuration-management-for)  
25. Building Scalable Applications with Python: Best Practices for Outsourced Teams \- Ellow.io, accessed June 17, 2025, [https://ellow.io/building-scalable-applications-with-python-best-practices-for-outsourced-teams/](https://ellow.io/building-scalable-applications-with-python-best-practices-for-outsourced-teams/)  
26. Model Context Protocol (MCP): A Guide With Demo Project \- DataCamp, accessed June 17, 2025, [https://www.datacamp.com/tutorial/mcp-model-context-protocol](https://www.datacamp.com/tutorial/mcp-model-context-protocol)  
27. Python Logging Guide: Proper Setup, Advanced Configurations, and Best Practices, accessed June 17, 2025, [https://edgedelta.com/company/blog/python-logging-best-practices](https://edgedelta.com/company/blog/python-logging-best-practices)  
28. How to Deploy LLMs with BentoML: A Step-by-Step Guide | DataCamp, accessed June 17, 2025, [https://www.datacamp.com/tutorial/deploy-llms-with-bentoml](https://www.datacamp.com/tutorial/deploy-llms-with-bentoml)  
29. Introducing Dolce – A Python Library for AI Development, accessed June 17, 2025, [https://discuss.python.org/t/introducing-dolce-a-python-library-for-ai-development/81697](https://discuss.python.org/t/introducing-dolce-a-python-library-for-ai-development/81697)  
30. Python Data Persistence Quick Guide \- Tutorialspoint, accessed June 17, 2025, [https://www.tutorialspoint.com/python\_data\_persistence/python\_data\_persistence\_quick\_guide.htm](https://www.tutorialspoint.com/python_data_persistence/python_data_persistence_quick_guide.htm)  
31. Python models | dbt Developer Hub \- dbt Docs, accessed June 17, 2025, [https://docs.getdbt.com/docs/build/python-models](https://docs.getdbt.com/docs/build/python-models)  
32. Building AI and Machine Learning Projects with Python \- Codewave, accessed June 17, 2025, [https://codewave.com/insights/building-ai-ml-projects-python/](https://codewave.com/insights/building-ai-ml-projects-python/)  
33. How to Secure a Python Server: Best Practices & TLS Setup \- Cyfuture Cloud, accessed June 17, 2025, [https://cyfuture.cloud/kb/howto/how-to-secure-a-python-server-best-practices--tls-setup](https://cyfuture.cloud/kb/howto/how-to-secure-a-python-server-best-practices--tls-setup)