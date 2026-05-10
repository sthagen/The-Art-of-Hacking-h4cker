# Lesson 6-5: Introducing WebMCP

> Student follow-along resources, key concepts, and references for this sublesson.

## Overview

**Lesson 6-4** introduced the **Model Context Protocol (MCP)** — how AI hosts attach to **servers** that expose tools, resources, and prompts (often on the backend or over the network). **WebMCP** is a separate but related idea: it targets the **browser** and **web pages**, letting sites **register structured tools** with the user agent so assistants can invoke **application-defined capabilities** instead of scraping the DOM or driving pixels.

Work on WebMCP sits alongside standards communities (for example the **W3C Web Machine Learning Community Group**) and browser vendors exploring **preview implementations**. The story through 2025–2026 is **rapid evolution**: always verify current browser docs and proposal text before you ship.

This sublesson frames **when WebMCP complements MCP**, how **permissions** and **user consent** fit in, and what to watch for in **security** and **fallback** design.

## Learning objectives

By the end of this sublesson you should be able to:

- Contrast **WebMCP** (browser / page surface) with **MCP** (host ↔ server integrations).
- Explain why exposing **typed tools** from a site can be safer and more reliable than unstructured UI automation.
- Identify **permission**, **least privilege**, and **human-in-the-loop** considerations when a page exposes capabilities to an assistant.
- Find authoritative sources (proposal docs, browser guidance) and treat WebMCP as an **evolving** API with flag-gated availability.

## Key concepts

### 1. Two layers: backend MCP and browser WebMCP

| Layer | Typical transport | What it connects |
| --- | --- | --- |
| **MCP** | stdio, Streamable HTTP, enterprise gateways | Host app ↔ MCP servers (GitHub, DB, internal APIs) |
| **WebMCP** | Browser APIs on the page | User session ↔ registered page capabilities |

They stack: your **coding agent** might use **MCP** for repo and CI tools while the **user** works in a web app that exposes **WebMCP** actions for checkout, approvals, or tenant-specific workflows.

### 2. What WebMCP tries to fix

Without an explicit capability surface, assistants either **guess** from HTML or automate **UI controls** — brittle, slow, and easy to get wrong when layouts change. Registering tools gives:

- **Stable contracts** — names, parameters, and semantics owned by the application.
- **Finer permission UX** — the browser can mediate access the way it already mediates camera or notifications.
- **Less accidental data exposure** — fewer reasons to paste entire DOM trees into the model.

### 3. Security and governance habits

Treat registered capabilities like **new attack surface**:

- **Scope** tools narrowly; validate arguments server-side even when the caller is “the assistant.”
- Pair sensitive operations with **HITL** patterns from the next sublesson (**Lesson 6-6**): approvals, audit trails, and rollback where needed.
- Provide **degraded modes** when WebMCP is unavailable so workflows still work for humans.

### 4. Practical stance for practitioners

- Read **vendor proposal and blog posts** alongside the **MCP spec** — WebMCP is not “MCP over HTTP” as a drop-in; it is its own integration surface.
- Prototype behind **preview flags**; avoid hard-coding assumptions about universal availability.
- Document which capabilities are **page-level** vs already covered by your existing **MCP servers** so teams do not duplicate or contradict backends.

## Why it matters / What's next

WebMCP matters because much of users’ work still happens in **web apps**. Giving agents a **first-class, consent-aware** interface there reduces fragile automation and aligns with how MCP already standardized backend tools.

The next sublesson, **Lesson 6-6: Human-in-the-Loop (HITL) and Human-on-the-Loop (HOTL) Strategies**, adds governance: when autonomous runs must pause or be monitored by people.

## Glossary

- **WebMCP** — Emerging browser-oriented pattern for pages to register structured tools for assistants (see current proposal and browser docs for exact API names).
- **MCP (Model Context Protocol)** — Open protocol for connecting AI hosts to servers that expose tools and resources (Lesson 6-4).
- **Typed tool / capability** — A named operation with a schema or contract, as opposed to ad-hoc HTML scraping.
- **Least privilege** — Granting assistants only the capabilities needed for the task.
- **Human-in-the-loop (HITL)** — Explicit human approval before a sensitive step (covered next).

## Quick self-check

1. In one sentence, how does WebMCP differ from hosting an MCP server?
2. Name one reliability benefit of registered tools versus DOM-only automation.
3. Why should sensitive WebMCP actions still be validated on the server?
4. Where should you look for the latest availability and API details?

## References and further reading

- Chrome for Developers — *When to use WebMCP and MCP.* https://developer.chrome.com/blog/webmcp-mcp-usage
- W3C Web Machine Learning CG — *WebMCP API Proposal (editor draft).* https://webmachinelearning.github.io/webmcp/docs/proposal.html
- Model Context Protocol — *Introduction (compare integration model).* https://modelcontextprotocol.io/

### Omar's resources and references (course-wide)

#### Foundational cybersecurity resources in O'Reilly

This section provides a curated list of resources that delve into foundational cybersecurity concepts, frequently explored in O'Reilly training sessions and other educational offerings.

##### Live training

- **Upcoming Live Cybersecurity and AI Training in O'Reilly:** [Register before it is too late](https://learning.oreilly.com/search/?q=omar%20santos&type=live-course&rows=100&language_with_transcripts=en) (free with O'Reilly Subscription)

##### Reading list

Despite the rapidly evolving landscape of AI and technology, these books offer a comprehensive roadmap for understanding the intersection of these technologies with cybersecurity:

- **[NEW: Agentic AI for Cybersecurity: Building Autonomous Defenders and Adversaries](https://www.oreilly.com/library/view/agentic-ai-for/9780135589861/).** Unlock the power of next generation AI agents to transform cybersecurity, business operations, and productivity. [Available on O'Reilly](https://www.oreilly.com/library/view/agentic-ai-for/9780135589861/)

- **[Redefining Hacking](https://learning.oreilly.com/library/view/redefining-hacking-a/9780138363635/)** — A Comprehensive Guide to Red Teaming and Bug Bounty Hunting in an AI-driven World. [Available on O'Reilly](https://learning.oreilly.com/library/view/redefining-hacking-a/9780138363635/)

- **[AI-Powered Digital Cyber Resilience](https://www.oreilly.com/library/view/ai-powered-digital-cyber/9780135408599/)** — A practical guide to building intelligent, AI-powered cyber defenses in today's fast-evolving threat landscape. [Available on O'Reilly](https://www.oreilly.com/library/view/ai-powered-digital-cyber/9780135408599/)

- **[Developing Cybersecurity Programs and Policies in an AI-Driven World](https://learning.oreilly.com/library/view/developing-cybersecurity-programs/9780138073992)** — Explore strategies for creating robust cybersecurity frameworks in an AI-centric environment. [Available on O'Reilly](https://learning.oreilly.com/library/view/developing-cybersecurity-programs/9780138073992)

- **[Beyond the Algorithm: AI, Security, Privacy, and Ethics](https://learning.oreilly.com/library/view/beyond-the-algorithm/9780138268442)** — Gain insights into the ethical and security challenges posed by AI technologies. [Available on O'Reilly](https://learning.oreilly.com/library/view/beyond-the-algorithm/9780138268442)

- **[The AI Revolution in Networking, Cybersecurity, and Emerging Technologies](https://learning.oreilly.com/library/view/the-ai-revolution/9780138293703)** — Understand how AI is transforming networking and cybersecurity landscape. [Available on O'Reilly](https://learning.oreilly.com/library/view/the-ai-revolution/9780138293703)

##### Video courses

Enhance your practical skills with these video courses designed to deepen your understanding of cybersecurity:

- **[Building the Ultimate Cybersecurity Lab and Cyber Range](https://learning.oreilly.com/course/building-the-ultimate/9780138319090/)** (video). [Available on O'Reilly](https://learning.oreilly.com/course/building-the-ultimate/9780138319090/)

- **[Build Your Own AI Lab](https://learning.oreilly.com/course/build-your-own/9780135439616)** (video) — Hands-on guide to home and cloud-based AI labs. Learn to set up and optimize labs to research and experiment in a secure environment. [Available on O'Reilly](https://learning.oreilly.com/course/build-your-own/9780135439616)

- **[Defending and Deploying AI](https://www.oreilly.com/videos/defending-and-deploying/9780135463727/)** (video) — Comprehensive, hands-on journey into modern AI applications for technology and security professionals, covering AI-enabled programming, networking, and cybersecurity; securing generative AI (LLM security, prompt injection, red-teaming); secure AI labs; AI agents and agentic RAG for cybersecurity. [Available on O'Reilly](https://www.oreilly.com/videos/defending-and-deploying/9780135463727/)

- **[AI-Enabled Programming, Networking, and Cybersecurity](https://learning.oreilly.com/course/ai-enabled-programming-networking/9780135402696/)** — Learn to use AI for cybersecurity, networking, and programming tasks with practical, hands-on activities. [Available on O'Reilly](https://learning.oreilly.com/course/ai-enabled-programming-networking/9780135402696/)

- **[Securing Generative AI](https://learning.oreilly.com/course/securing-generative-ai/9780135401804/)** — Security for deploying and developing AI applications, RAG, agents, and other AI implementations; incorporate security at every stage of AI development, deployment, and operation. [Available on O'Reilly](https://learning.oreilly.com/course/securing-generative-ai/9780135401804/)

- **[Practical Cybersecurity Fundamentals](https://learning.oreilly.com/course/practical-cybersecurity-fundamentals/9780138037550/)** — Essential cybersecurity principles. [Available on O'Reilly](https://learning.oreilly.com/course/practical-cybersecurity-fundamentals/9780138037550/)

- **[The Art of Hacking](https://theartofhacking.org)** — Over 26 hours of training in ethical hacking and penetration testing (e.g., OSCP or CEH prep). [Visit The Art of Hacking](https://theartofhacking.org)

##### Certification related

- **CompTIA PenTest+ PT0-002 Cert Guide, 2nd Edition** — [Available on O'Reilly](https://learning.oreilly.com/library/view/comptia-pentest-pt0-002/9780137566204/)

- **Certified Ethical Hacker (CEH), Latest Edition** — Very comprehensive (19+ hours). [Available on O'Reilly](https://learning.oreilly.com/course/certified-ethical-hacker/9780135395646/)

- **Certified in Cybersecurity - CC (ISC)²** — [Available on O'Reilly](https://learning.oreilly.com/course/certified-in-cybersecurity/9780138230364/)

- **CCNP and CCIE Security Core SCOR 350-701 Official Cert Guide, 2nd Edition** — [Available on O'Reilly](https://learning.oreilly.com/library/view/ccnp-and-ccie/9780138221287/)

- **CEH Certified Ethical Hacker Cert Guide** — [Available on O'Reilly](https://learning.oreilly.com/library/view/ceh-certified-ethical/9780137489930/)

##### Additional resources

- **Hacking Scenarios (Labs) on O'Reilly** — Cloud-based labs; no local install. [https://hackingscenarios.com](https://hackingscenarios.com)

- **Personal blog** — [becomingahacker.org](https://becomingahacker.org)

- **Cisco blog** — [blogs.cisco.com/author/omarsantos](https://blogs.cisco.com/author/omarsantos)

- **GitHub repository** — [hackerrepo.org](https://hackerrepo.org)

- **WebSploit Labs** — [websploit.org](https://websploit.org)

- **NetAcad Ethical Hacker Free Course** — [NetAcad Skills for All](https://www.netacad.com/courses/ethical-hacker?courseLang=en-US)
