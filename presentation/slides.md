---
marp: true
theme: default
paginate: true
---
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.16.1/+esm';
  mermaid.initialize({ startOnLoad: true });
</script>

# Key Design Considerations

How do we get a machine to have a certain state?

Pattern A: A machine agent serves as the administrator's "extended hand", and runs code decided on by a central administrator-controlled system

<div class="mermaid">
flowchart LR
    Admin -->|enters commands into| ControlSystem[control system]
    ControlSystem -->|sends commands to| Agent[agent]
    Agent -->|executes commands on| Machine[machine]
</div>

Pattern B: A desired machine state for a whole fleet of machines is defined. The machine is pointed at the right machine state channel, and then keeps itself up to date with the machine state channel.

<div class="mermaid">
flowchart LR
    Updater[updater] -->|replaces system on| Machine[machine]
    Updater -->|fetches latest declaration from| ImageStream[image stream]
    Admin[admin] -->|updates declaration on| ImageStream
</div>

We are choosing to avoid using Pattern A. Instead, we extend Pattern B so that it solves all the problems relevant to os2fri that require interactions with individual machines.

**Why?**
- ensures that there is a approved system state which is described in a single location (single source of truth) -> leverer på Pålidelighed, Sikkerhed, Effektiv drift, Lovgivningsmæssig efterlevelse, Driftskontinuitet
we want to avoid:
- machines start to slightly deviate from each other in ways that are only understood by the person who performed the change
- undocumented changes to some machines cause update compatibility problems
- unauthorized actors breach into the all-powerful actor, and perform dangerous changes to the system
- knowledge about how systems work gets lost when municipalities change suppliers, or key employees change job
we want:
- every machine can be returned to its desired state
- it is easy to collect documentation on which software is run on the fleet and how it is configured

We will now describe how we solve individual problems within the confines of Pattern B.

---

# Note on problem speculations

A software system should be designed to solve specific problems.
During the design process for os2fri, one Product Owner of a related project was part of the team, but otherwise, we were not able to talk to the actual participants of the system.
Therefore, we are _speculating_ about which problems are important enough to discuss here.

During the actual development, user representatives must be included in the development process, so that the actual problems can be investigated and prioritized.

While thinking about how to implement os2base, the following problems appeared important enough to address early on:

---

# Problem: How do we have most users use the same locked-down system, while giving users flexibility?

We predict these problems:

- As admin, I want users to use the system within predictable confines and I want to be able to re-use the system setup for as many systems as possible (Driftseffektivitet)
- As user, I want to be able to adjust the system to my needs, and I don't want to be forced to use an inadequate solution that some third-party picked for me

---

## Solution Design

- We define groups of machines with similar configurations like this: system declarations inherit each other, and sub-systems of existing systems can be defined

Example: Indskolings-PC at Aarhus Municipality:

<div class="mermaid">
flowchart LR
    Machine["Machine"]
    subgraph ownerOS2Base ["OS2Base"]
        OS2BaseDecl["OS2Base declaration"]
    end
    subgraph ownerOS2Skole ["OS2Skole"]
        OS2SkolePcDecl["OS2SkolePC declaration"]
    end
    subgraph ownerAarhus ["Aarhus Municipality"]
        SkoleAtAarhusDecl["Skole@Aarhus declaration"]
        IndskolingAtAarhusDecl["Indskoling@Aarhus declaration"]
    end
    Machine -->|follows image stream from| IndskolingAtAarhusDecl
    IndskolingAtAarhusDecl -->|inherits from| SkoleAtAarhusDecl
    SkoleAtAarhusDecl -->|inherits from| OS2SkolePcDecl
    OS2SkolePcDecl -->|inherits from| OS2BaseDecl
</div>

To avoid having one declaration per machine:

- the administrator predefines a confine within which the user can make choices about their system
- the choice and design of confines is specific to the problems users might face
-> subsystem that upholds certain promises

For example: User "I want to be able to install software that is relevant to my use case"
Solution: Provide a list of apps that are allowed to be installed on the system. These apps must not change any of the parameters that the admin relies on (existing sandboxing solutions for this exist)

Another example: User "I want to be able to use the right printer at my office"
Solution: Make the system agnostic to which specific printer is being used by a user. A machine can gain access to a long list of printers, and potential permission rights are handled at the printer/network level.

---

# Problem: How do we know the state of the fleet?

Predicted Problems: (admin)

- I need to know whether the machines are still alive
- I need to know which users have authenticated on the machine
- I need to know which networks the machines have been connected to

Bigger picture:

- I need to know who is using which machine at a given time, and whether unauthorized individuals are using the machine.

---

## Solution Design

- each machine exposes standardized observability data
- there's an observability central system that collects all this data
- observability data is converted into a standardized format

So the information flow becomes:

<div class="mermaid">
flowchart LR
    Machine -->|pushes standardized usage data to| ObservabilitySystem[Observability system]
    Machine -->|pulls image stream patches from| ImageRegistry[Image Registry]
</div>

Required machine components:

- a component that collects machine behavior, converts this behavior into the standardized usage data format, and sends it to the observability system
- a component that regularly checks whether there are new image patches, downloads them, and applies them to the machine

---

# Problem: How can system configuration be a seamless experience?

🧑‍💻 Admin
👩‍💼 CISO

Predicted problems:

|🧑‍💻"When I want to change the configuration of the fleet, I know I can find the right setting in the configuration interface. It's my one-stop-shop for any sort of configuration."|🧑‍💻"I'm sure they're using all sorts of complicated technology to make this system work, but luckily, I don't have to learn about any of it to change the configuration."|
|----|----|
|👩‍💼"Recently a software was installed on the fleet where I didn't really know why it is there. Luckily, I could easily check who added this configuration, who approved it, and when the change was made."|👩‍💼"I want to be sure that none of my employees can change the fleet config without having someone else review the change."|

- As admin, I do not want to have to navigate between different admin interfaces just because different teams are working on the platform's backend
- As admin, I want to use a configuration interface that makes sense to me, and that does not force me to learn technological details that are not relevant to my work
- As CISO, I want to know which configurations in the declarations have been set when, and by which person.
- As CISO, I want to make sure that an admin can only roll out a change to the fleet, after their change has been reviewed by another admin.

---

## Solution Design

- configuration management is based on an existing VCS solution like Git. **Why?** This solution gives us sporbar historik, tilbagerulning, gennemgang og godkendelse af ændringer for free
- as part of os2fri, a thin layer on top of Git is created, which makes configuration easier for local admins
- os2base provides the UI logic for the configuration interface, and investigates the appropriate visual/experience design language
- the UI logic converts a standardized intermediary format into a UI
- the individual os2fri products expose configuration items in the standardized intermediary format

---

# Problem: How can we change the system configuration of an existing system from remote?

Predicted problems:

- As an admin, I want to be able to change the installation channel of a system without having to be at the location
- As an admin, I want to be able to reset the user data on the machine without having to be at the location ("powerwash")

---

## Solution Design

Specific manipulation pathways need to be investigated ahead of time, and then logic for those manipulations is added to the system.

Because a specific system only ever pulls the configuration that other systems are also pulling, the problems can be solved like this:

- specific manipulation paths are pre-configured into the image
- with every image update, a list gets downloaded by all machines, and this list contains the IDs of all machines onto which the manipulation is to be applied
- this means that every machine does have some kind of unique and unchangeable identification

**Important:** Per-machine changes only move the machine to the state of a known, approved image stream.

---

# Admin and User Experience

Problems:

- Which actors are relevant to the system? What are their specific needs and user context?
- How can we make the barrier to entry low for desktop end users?
- How do we make the complexity/error risk low for administrators?
- How do we handle hardware compatibility and hardware choice?
- Which regulatory requirements are relevant for UI choices?
- How do we ensure we get user input early and where it matters?

---

## Which actors are relevant to the system? (example)

<div style="display: flex; align-items: center; gap: 40px;">

<div style="flex: 1;">

### Recommendations

- Consider A, B, C
- Prioritize xyz

### Trade-offs

- If A, then this will mean B

</div>

<div class="mermaid" style="flex: 1">
flowchart LR
    A --> System
    System --> B
    C --> D
    D --> System
</div>

</div>

---

# Standards & Compliance

Problems:

- How do we ensure we align with data regulations (esp. NIS2/GDPR/CRA)?
  - How do we keep cost low?
  - Quality standards
- How do we ensure the security of our supply chains?
- How do we ensure our solution is digitally sovereign?
- How do we ensure our solution is secure?
- How do we handle compliance needs or implementation differences that differ by municipality?

---

# Shared Functionality

Problems:

- Which software components can address shared problems across os2fri projects and should be part of os2base?
- In an operating system context, which components should be part of os2base?
- How do we isolate administration access from daily user access rights?
- How do we handle software updates?
- How do we allow individual instances to customize their installation?

---

## os2fri Domain Model

<https://example.com>

---

# Process and Humans

Problems:

- How do we distribute the development responsibilities?
- Which role do we play in ensuring users/local admins receive appropriate technical support?
- What's the appropriate level of flexibility that we should give municipalities in their configuration?
- How do we prevent misconfigurations?
- How do we make it easy for stakeholders to use the platform in the way we intend?
- How do we ensure that potential changes in stakeholders' workflows will be tolerated by them?

---

# Shared operations model

Problems:

- How do we ensure that different suppliers are delivering equivalent and interchangeable solutions?
- Which administration tasks lie with the supplier vs. which ones lie with the local administrator?
- What conditions should be given for a healthy supplier ecosystem in os2base?
