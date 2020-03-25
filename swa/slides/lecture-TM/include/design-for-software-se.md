class: center, middle
# .red[Design] for Software Security Engineering

---
class: center, middle
# Threat and Design\*

![bad threat model](http://imgs.xkcd.com/comics/authorization.png)

.footnote[
\* Recommended by a Prior Student and [Bruce S.](https://www.schneier.com/blog/archives/2013/04/xkcd_on_a_bad_t.html)
]

---

# Systems Security Engineering

### .orange[Problem] Context
A sufficiently complete understanding of the problem

### .green[Solution] Context
<u>Transforms the security requirements into design requirements for the system</u>

### .red[Trustworthiness] Context
Evidence-based demonstration, through reasoning, that the system-of-interest is deemed trustworthy

???
The framework is independent of system type and engineering or acquisition process model and is not to be interpreted as a sequence of flows or process steps but rather as a set of interacting contexts, each with its own checks and balances

---
class: middle
# Security Analysis during Design

## Start with a Data-flow perspective
- **Most attacks come through .red[data]**
- **Attacks gravitate towards .red[data]**
- .blue[**Control flow**]: less relevant  
to analyze security in design and architecture

---
class: middle
# Data Flow Diagrams (DFD)
- A structured, concrete artifact to _discuss_ design:  
Conceptualization, Changes or Re-design
- Keeps designer focused on design issues

---
class: middle
- **Practical Relevance**: _"Applying a structured approach to threat scenarios during design helps a team .red[more effectively and less expensively] identify security vulnerabilities, determine risks from those threats, and establish appropriate mitigations"_ [Microsoft SDL](https://www.microsoft.com/en-us/SDL/process/design.aspx), [Threat Modeling](https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling)

---
class: middle
# Data Flow Diagrams (DFD)

## DFDs are visual representations of .red[**data flows**] through components of a software program
- Components of a software program:  
.green[Data Source] &#8594; .orange[Data Transformations] &#8594; .red[Data Sink]
- All control flows are abstracted into _processes_  
that perform data transformations

???

- Data flows provide a significantly simple system/software view when compare to control flow.

---

class: middle
# DFD Elements
1. External Interactors
1. Processes
1. Data Stores
1. .red[Trust Boundaries]
1. Data Flows

---

# DFD Elements

.left-column[
## External Interactor/Entity
- .red[_Uncontrollable_] by the codebase of interest but can interact with it
- Generates data (Source)
- Receives data (Sink)

## Examples    
People, External systems, External APIs
]

--

.right-column[
## Representation
![example](images/externalentity.svg)
## Naming
.red[Noun phrase] that describes the entity
]

???
## NOT the focus of threat modeling
---
# DFD Elements

.left-column[
## Process (Code)
- A collection of .red[code]
- Codebase of interest that is being threat modeled
- Transforms input data into output data

## Examples     
- Code, Native code, Executables, Libraries, Process execution scope
]

--

.right-column[
## Representation
![example](images/process.svg)  
## Naming
- .red[Noun phrase] used to refer to the codebase
- .red[Include process number]
]

---

# DFD Elements

.left-column[
## Data Store
- Place to hold data  
- Storage for a single process or transfer between processes  

## Examples
Files, Registry keys, Databases, Shared memory, Message Queue
]

--

.right-column[
## Representation
![example](images/datastore.svg)
## Naming
- .red[Noun phrase], but plural
]

---

# DFD Elements

.left-column[
## Data Flow
- Flow of data between External Interactors, Processes and Data Stores  
- Contracts between DFD elements

## Examples
Cookie, Form data, Response page, Credentials, etc.
]

--

.right-column[
## Representation
![example](images/dataflow.svg)
## Naming
- .red[Noun phrase] that describes the application data being transferred
]

---

# DFD Elements

.left-column[
## Trust Boundary
- Intersects .red[data flows]
- Component on one side doesn't trust the one on the other side

## Examples  
- Data flows from one privilege level to another  
- External entities and processes with different trust levels
]

--

.right-column[
## Representation
![example](images/boundary.svg)
## Naming
- Describes the boundary placement
]

---
class: middle
# DFD Levels

## Levels are hierarchically related
- Based on the granularity of the processes
- Levels 0, 1, 2...

---
class: middle
## DFD Level 0 Example
- .red[**Level 0:**] Single process represents the whole system  
Very high-level; entire component / product / system.  
Show how the system interacts with the outside world.

![Example of DFD Level 0](images/dfd0.svg)

---
class: middle
## DFD Level 1 Example
- .red[**Level 1:**] Major processes and data stores identified   
High level; **single** feature / scenario

![Example of DFD Level 1](images/dfd1.svg)

---
class: middle
## DFD Level 2 Example
- .red[**Level 2:**] Detailed subcomponents of processes  
Low level; detailed features of a **single** feature / scenario

![Example of DFD Level 2](images/dfd2.svg)

---
class: middle

# How to start DFD Construction?

## Step 1
### Start with a Level 0 diagram for a use case
- A single process (represents the system, no data stores)
- Identify all External Interactors (EI)
- Draw data flows to connect EI & the process
---

class: middle
# Playsound API

"_The PlaySound API takes as input a string which represents either a WAV filename or an alias.  If the input is an alias, the PlaySound API retrieves data from the registry under HKCU to convert the alias into a filename.  Once the filename is determined, the PlaySound API opens the WAV file specified and reads the two relevant pieces from the file: the WAVEFORMATEX that defines the type of data in the file and the actual audio data.  It then hands that data to the audio rendering APIs._"

--

## Develop a Level 0 DFD*

.footnote[*_[Class exercise](https://docs.google.com/presentation/d/1dYBlbTcs3fBs-hawHgdJjL2xGrOFnrSOVpsB2pTGHNc/edit?usp=sharing)_]

---

class: middle
# DFD Construction
## Step 2
### Transition to a Level 1 diagram
- Breakdown the single Level 0 process into major processes and related data stores .red[for a single feature / scenario]
- Check your work
- Can you tell a story without edits?
- Does it match reality?

---
class: middle
# DFD Construction
## Step 3
### Add trust boundaries that intersect data flows
### Trust boundary placement
- Threads are often inside a trust boundary.   
They share the same privileges, rights, identifiers and access    


- Processes talking across a network may create a secure channel, but .red[they’re still distinct entities.]   

---
class: middle
# DFD Construction
## Step 4
### Iterate over processes and data stores
- Break them down if more detail needed to explain .red[_security impact of the design_]


- Break them down if an object crosses a trust boundary. For example, a remote procedural call (RPC)


- Break them down if you use words like “sometimes” and “also” in your story

---
class: middle
# DFD Construction
## Step 5: Check the diagram for sanity (1)
### Data stores
- Two data stores should not be connected with data flows directly. They are static entities.
- External interactors should not directly interact with data stores. Their data representation formats are different.
- Data stores should have an input flow
- Try to locate data sinks, whenever possible

### Data Flows
- Attached to at least one Process or External Interactor


---
# DFD Construction
## Step 5: Check the diagram for sanity (2)
### External Interactors
- Avoid data flows between External Interactors. They cannot be observed by the system.
- Data always comes from External Interactors.

### Processes in Level 2 and beyond
- Carefully think about direct dataflows between two separate processes on a single machine.   
- Use intermediate data stores such as message queues or domain sockets.
---
# DFD Construction
## Step 6
### Simplify
- Consolidate data flows that always flow together


- All processes need appropriate _inputs_ to generate _outputs_. No outputs without inputs!


- Avoid partitioning processes based on control logic.  
Partition processes that perform multiple functions and they exist in different process spaces.


- DFDs do not typically show time dependencies.   
If processes can communicate directly they are assumed to be synchronous!
---
class: middle
# DFD Modeling Summary
- DFDs depict data flows from sources to sinks with transformations along the way.   
Trust boundaries intersecting these data flows.


- Hierarchical structuring (Levels) allows system analysis at different levels of abstraction

???
![Example](images/playsoundthreatmodel.png)  
http://blogs.msdn.com/b/larryosterman/archive/2007/09/13/threat-modeling-again-analyzing-the-threats-to-playsound.aspx

---
class: middle, center
# Threat Identification
DFDs allows potential threats to be .red[automatically generated]!


---

class: middle
# Microsoft STRIDE Threats

.left-column[
## Threat
- .red[S]poofing
- .red[T]ampering
- .red[R]epudiation
- .red[I]nformation Disclosure
- .red[D]enial of Service
- .red[E]levation of Privilege
]

---

class: middle
# Microsoft STRIDE Threats

## Spoofing
* A process or entity is something other than the claimed identity

## Tampering
* Act of altering bits

## Repudiation
* Deny that something happened or an action was taken

---
class: middle
# Microsoft STRIDE Threats

## Information Disclosure
* Information can be read by an unauthorized party

## Denial of Service
* Process or data store not able to process incoming requests

## Elevation of Privilege
* A user can increase capability or privilege by taking advantage of an implementation bug

---
class: middle

# STRIDE Per .red[Interaction]
## Focus on Interactions
- Interaction:   
A source and target element connected by a data flow
![Example of DFD Level 1 Interaction](images/interactiondfd1.svg)

---
class: middle

# STRIDE Per .red[Interaction]
## For each interaction apply STRIDE
- For each STRIDE threat identify the attacker controlled element and the attacked element
- For data flows inside a single process space,   
don’t worry about T, I, or D
- Prioritize interactions that _cross trust boundaries_
???
## Significant reduction in number of threats to be analyzed
---
class: middle

# Trust boundaries
## Trusted/high code reading from untrusted/low
- Look for Tampering threats

## High code writing to low
- Errors may result in Information Disclosure
---
class: middle

# Avoid Distractions
## Applications can't do much here:
- The computer is infected with malware
- Someone removed the hard drive and tampers
- Admin is attacking user
- A user is attacking himself

## Applications can’t address any of these   
(unless you’re the OS)

---
class: middle

# Practice

![OWASP](https://www.owasp.org/images/1/16/Data_flow2.jpg)

#### How many interactions? How many are high priority?

---

class: middle

# Practice
![example](images/dfd.png)

#### How many interactions? How many are high priority?

---
class: middle
# Validate the Threat Model

1. Do the threats consider misuse cases?
1. Does the diagram match final code?
1. Are enough threats enumerated?
1. Has Test / QA reviewed the model?
Testers often finds issues with threat model or missing details
1. Is each threat mitigated?
1. Are mitigations done right?  (Assurance case, possibly)

---
class: top
# Tool Support

--

## Automation
- Much of this analysis can be automated using simple rules based on the diagram structure
- Tool support

## Traceability
- Would be nice to link all analysis along with the diagram
- A bit more than a PPT, Visio or Lucidchart!

---
class: middle
## [Microsoft Threat Modeling Tool (TMT)](https://aka.ms/tmt)
![toolsupport](images/toolsupport.png)
---
class: middle
# .center[Threat Mitigation]

.footnote[Why bother if you create a great model, identify lots of threats, and .red[stop!]]

---
class: middle
# Threat Mitigations
## Four ways to address each threat
1. Redesign to eliminate
1. Apply standard mitigations  
What have similar software packages done and how has that worked out for them?
1. Invent new mitigations (.red[risky!])
1. Accept vulnerability in design  
Risk acceptable, but must be verified and approved

---
class: middle
# STRIDE Controls

.left-column[
## Threat
- .red[S]poofing
- .red[T]ampering
- .red[R]epudiation
- .red[I]nformation Disclosure
- .red[D]enial of Service
- .red[E]levation of Privilege
]

.right-column[
## Control
- Authentication
- Integrity
- Nonrepudiation
- Confidentiality
- Availability
- Authorization
]

---

class: middle
# Standard Mitigations

## Spoofing
### Require authentication
- Data source
- Code integrity

### Validation of input read from the data source
- Normalization before neutralization
- Avoid recursion bombs

---
class: middle
# Standard Mitigations

## Tampering

### Integrity Checking
- Digital signatures and message authentication codes
- Access Control Lists (ACLs) for data at rest

### Validation of input read from the data source
- Normalization before neutralization
- Avoid recursion bombs

---
class: middle
# Standard Mitigations

## Repudiation
- One-way log and audit generation mechanism
- Strong authentication for logging

## Information Disclosure
- ACLs
- Encryption with good key management and protocols
---
class: middle
# Standard Mitigations

## Denial or service
- ACLs to protect the contents of files from being removed or modified
- Firewall rules to protect against some network based attacks
- Use disk and processor quotas to prevent excess disk or CPU consumption

---
class: middle
# Standard Mitigations

## Elevation of Privilege
- Input validation
- Input validation
- Input validation
- ACLs, Roles, Groups
- Privilege dropping
- Least privilege

---
# How to ignore threats?
## No requirement
- There are no requirements that the &lt;&lt;element&gt;&gt; protect against &lt;&lt;STRIDE&gt;&gt; threat

## Irrelevant
- This threat does not exist, so we don't care about &lt;&lt;STRIDE&gt;&gt; threat to the &lt;&lt;element&gt;&gt;

## Not applicable
- &lt;&lt;STRIDE&gt;&gt; threat does not apply to this &lt;&lt;element&gt;&gt;

---

# Making this Fun

## Elevation of Privilege Card Game
- Ease developers into doing threat modeling
- [How to play](http://social.technet.microsoft.com/wiki/contents/articles/285.elevation-of-privilege-the-game.aspx)
- [Card Images](https://robinagandhi.github.io/swa/slides/lecture-4/images/eopcardcameimages.pdf)
- [Other SDL developer resources](https://www.microsoft.com/en-us/securityengineering/sdl/resources)

![EOP](https://c.s-microsoft.com/en-us/CMSImages/EoP_game_screen_shot.jpg?version=4a082487-9fb4-7dd9-ed9f-e79c888c2df4)


---
class: middle
# More about threat modeling

## Blogs
- Threat Modeling Tool [User Guide](https://docs.microsoft.com/en-us/azure/security/develop/threat-modeling-tool)
- Bruce Schneier on [threat modeling](http://www.schneier.com/blog/archives/2007/10/threat_modeling.html)

## Practice Diagrams
- Microsoft [Readings](https://msdn.microsoft.com/en-us/library/aa562036.aspx)

## Threat Modeling in Practice
- [SAFECode Tactical Threat Modeling](https://safecode.org/safecodepublications/tactical-threat-modeling/)

---
# Sources

- This presentation is borrows a lot from Microsoft training materials on threat modeling
- Many sources for Data flow diagrams

---

class: center, middle
# Threat Modeling In-Class Demonstration

---
class: middle

# Step 1
- Download and install [Microsoft TMT](https://aka.ms/threatmodelingtool)

---

class: middle
# Step 2
- Build a DFD for the Playsound API in TMT
- Examine the threats identified

---
class: center, middle
# Discussion?
![that-dont-make-no-sense](https://media.giphy.com/media/DO5JobrylWL7i/giphy.gif)
