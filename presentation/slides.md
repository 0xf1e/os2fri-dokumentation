---
marp: true
theme: default
paginate: true
---
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>

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

## OS2Fri Domain Model

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
