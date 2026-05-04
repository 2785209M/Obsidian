## **1. Project Management**

**(a)** Explain the purpose of a Scrum Sprint Planning meeting, contrasting its purpose with that of a Backlog Grooming (Refinement) session. **[4]**

**(b)** Based on the scenario, the team’s approach to planning their second sprint is deeply flawed. Identify **six** aspects of the Sprint Planning meeting, task estimation, or work allocation that contradict Agile/Scrum best practices. For each problem, explain why it is an issue and propose a better approach that the team should adopt. **[12]**

**(c)** During the sprint review for Sprint 1, Jess (the marketing manager) states she wants to use the system for building a garden shed. Given that Sprint 1 focused purely on decorating an indoor room, explain why committing to the garden shed feature immediately in Sprint 2 might be risky, and describe an Agile technique the team could use to mitigate this risk before building the feature. **[4]**

---

## **2. Requirements Engineering**

**(a)** Consider the following draft user stories created by the team for the new Chat-DI-AI application:

1. _As a developer, I want to use the shopping app REST API so that I can add items to the shopping cart._
    
2. _As a customer, I want to discuss my project with Chat-DI-AI, enter my room measurements, get color suggestions, pick a color, and generate a shopping list so that I can decorate my son's bedroom._
    
3. _As a customer, I want Chat-DI-AI to generate responses quickly so that I don't get bored waiting._
    
4. _As a customer, I want to add items to my shopping cart._
    
5. _As the marketing manager, I want the UI to not look ugly so that customers will enjoy using it._
    

Choose **all five** of the user stories and:

(i) Explain why the story violates one of the **INVEST** criteria (choose a different primary violation for each story).
(ii) Propose a revised user story (or stories) that complies with the INVEST criteria. 
**[15]**    

**(b)** Suppose that connecting the LLM to the full product catalogue (decorating, plumbing, timber, etc.) is highly complex. Jess wants an initial Minimum Viable Product (MVP) released to customers as soon as possible to generate buzz. Propose a **MoSCoW** prioritization for the features required to deliver an MVP focused _only_ on the "room decoration" journey described in the scenario. Justify your answer. **[5]**

---

## 3. Software Architecture

The new Chat-DI-AI system needs to coordinate between the cross-platform UI, the external Large Language Model (LLM) service, the existing Shopping Platform's REST API (for cart management), and the Catalogue API (for product searches).

**(a)** From the architectural patterns you have studied, choose an appropriate pattern for integrating the Chat-DI-AI frontend with the various backend services (LLM, Catalogue, Shopping Cart). Explain why this pattern is a suitable solution for the design problem. **[3]**

**(b)** Explain how you would tailor the chosen architectural pattern to fit the specific needs of the Chat-DI-AI scenario. **[3]**

**(c)** A colleague proposes an alternative approach where the Chat-DI-AI client application (the cross-platform UI) communicates _directly_ with the external LLM Service, the internal Catalogue API, and the internal Shopping Cart API without any middleware. Identify and explain **three** significant drawbacks of this direct-communication approach compared to the architecture you chose above. **[6]**

**(d)** Later, the team decides they do not want to be locked into a single LLM provider. They want the ability to easily swap between different third-party LLMs (e.g., OpenAI, Anthropic, local models) without rewriting the core application logic. Explain how the **Plugin (or Adapter)** architectural pattern could be used to accommodate this new requirement. **[3]**

---

## **4. Software Maintenance and Testing**

Study the Behaviour Driven Development (BDD) feature file provided in the scenario and answer the following questions.

**(a)** BDD feature files should serve as readable documentation as well as automated tests. Identify **three** "bad smells" or anti-patterns in the provided Cucumber feature file. For each smell:

(i) Explain why it makes the test suite harder to maintain or understand.
(ii) Propose a refactoring to remedy the smell (you may write out the refactored Gherkin code).
(iii) Explain how the refactoring improves the maintainability of the test suite. **[12]**

**(b)** A colleague claims that because all four scenarios pass, the discount code logic of the shopping cart is 100% tested and ready for production. Explain to your colleague why relying purely on the number of passing scenarios or code coverage can be a misleading measure of the effectiveness of a test suite. **[4]**

**(c)** Explain the following terms in the context of mutation testing: 
(i) Mutation 
(ii) Mutant 
(iii) Survivor 
(iv) Effectiveness 
**[4]**