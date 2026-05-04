### **1. Project Management**

**(a) Sprint Planning vs. Backlog Grooming**

- **Sprint Planning Meeting:** This meeting occurs at the very beginning of a sprint. Its purpose is for the team to define the Sprint Goal and select items from the top of the Product Backlog to determine _what_ will be delivered. The team then breaks these items down into technical tasks to determine _how_ they will achieve the goal, forming the Sprint Backlog.
    
- **Backlog Grooming (Refinement):** This is an ongoing activity that typically occurs mid-sprint. Its purpose is to look ahead at future Product Backlog items to ensure they are well-understood, clearly written (often as user stories), accurately estimated, and prioritized by the Product Owner so that they are "ready" for future Sprint Planning meetings.
    

**(b) Agile Anti-Patterns in the Scenario**

1. **Working separately:** The team decided to work in silos to be "more efficient."
    
    - _Why it's bad:_ Agile promotes cross-functional collaboration and collective code ownership. Silos create bottlenecks and reduce knowledge sharing.
        
    - _Alternative:_ The team should use practices like pair programming or mob-programming (as they successfully did in Sprint 1) to tackle features together.
        
2. **Estimating in "weeks" (absolute time):** Jane assigned estimates in weeks.
    
    - _Why it's bad:_ Time-based estimates for complex tasks are notoriously inaccurate due to unforeseen complexities.
        
    - _Alternative:_ The team should estimate using relative sizing, such as Story Points or T-shirt sizes.
        
3. **Senior developer estimating alone:** Jane applied estimates based purely on her judgment.
    
    - _Why it's bad:_ This ignores the perspective and experience of the rest of the team, and prevents junior developers from understanding the scope of the work.
        
    - _Alternative:_ The entire team should estimate collaboratively using a technique like Planning Poker.
        
4. **Tasks are too large:** Issues 2 and 3 are estimated at 2 weeks.
    
    - _Why it's bad:_ A task that takes the entire sprint cannot be tracked effectively during daily stand-ups and carries a huge risk of not being completed.
        
    - _Alternative:_ The team must break these massive user stories down into smaller, actionable tasks (e.g., taking no more than 1-2 days each).
        
5. **Everything is a "Must" priority:** All sprint tasks are labeled as MoSCoW "Must."
    
    - _Why it's bad:_ MoSCoW is generally used for release planning, not Sprint Backlogs. If everything in a sprint is a "Must," the team has no flexibility if a task takes longer than expected.
        
    - _Alternative:_ The Sprint Backlog itself represents the team's commitment. Prioritization should dictate _what_ makes it into the sprint, but once in, the team works sequentially based on value.
        
6. **Tasks are technical descriptions rather than User Stories:** e.g., "Link LLM to catalogue."
    
    - _Why it's bad:_ This focuses on the implementation rather than the value being delivered to the user.
        
    - _Alternative:_ Reframe the backlog items as user stories focused on business value (e.g., "As a customer, I want Chat-DI-AI to recommend specific products so that I can add them to my cart").
        

**(c) Mitigating Risk for the Garden Shed Feature**

- **The Risk:** Building a shed involves outdoor construction, complex materials (lumber, concrete), and safety regulations, which is a massive leap from painting an indoor wall. The domain is highly complex and uncertain.
    
- **Agile Technique:** The team should run an **Architectural Spike** (a time-boxed research task). During the sprint, a developer investigates the feasibility, data models, and API requirements for outdoor projects to gather information before committing to building the actual user story in a future sprint.
    

---

### **2. Requirements Engineering**

**(a) INVEST Criteria Evaluation and Revision**

1. _Story 1 (Developer API task):_ * **Violation:** Violates **Independent** or **Valuable**. It focuses on technical implementation details rather than end-user value.
    
    - **Revised:** "As a customer, I want to add AI-recommended materials directly to my shopping cart so that I can purchase them seamlessly."
        
2. _Story 2 (The massive user journey):_
    
    - **Violation:** Violates **Small**. It is an "Epic" that encompasses the entire user journey and is far too large to complete in a single iteration.
        
    - **Revised:** "As a customer, I want to input my room measurements so that the chat agent can accurately calculate the required amount of paint."
        
3. _Story 3 (Generate responses quickly):_
    
    - **Violation:** Violates **Testable**. The word "quickly" is subjective; developers and QA cannot verify if the system has met this requirement.
        
    - **Revised:** "As a customer, I want Chat-DI-AI to generate text responses in under 3 seconds so that my conversation feels natural."
        
4. _Story 4 (Add items to cart):_
    
    - **Violation:** Lacks a rationale, meaning it violates the goal of being **Valuable** (no "so that..." clause).
        
    - **Revised:** "As a customer, I want to manually adjust the AI-generated shopping cart so that I can remove items I already own."
        
5. _Story 5 (UI not looking ugly):_
    
    - **Violation:** Violates **Testable**. "Not ugly" is a subjective opinion.
        
    - **Revised:** "As the marketing manager, I want the Chat-DI-AI interface to utilize the brand's official CSS design system so that it visually matches our existing storefront."
        

**(b) MoSCoW Prioritization for the MVP**

- **Must Have:** The chat interface, integration with the LLM for Q&A, and the ability for the LLM to output a plain-text shopping list. _(Justification: This is the core value proposition. A plain text list proves customers actually want to use an AI for DIY planning without requiring complex API integrations)._
    
- **Should Have:** Ability to parse the text list and search the existing store catalogue to provide product links.
    
- **Could Have:** Generating image samples of paint colors in the chat UI.
    
- **Won't Have (for now):** Direct API integration to automatically populate the shopping cart; email integration; complex logic for sheds/plumbing. _(Justification: These are optimization features. If the MVP fails, building the automatic cart integration would be wasted effort)._
    

---

### **3. Software Architecture**

**(a) Architectural Pattern**

- **Pattern:** **API Gateway** (or **Broker** pattern).
    
- **Explanation:** The Chat-DI-AI UI needs data from the external LLM, the internal Catalogue API, and the internal Shopping Cart API. Having the mobile/web client communicate with all three directly creates tight coupling, security risks, and complex frontend code. An API Gateway centralizes this orchestration.
    

**(b) Tailoring the Pattern**

- The API Gateway will sit between the UI and the backend services. When Jim sends a chat message, the Gateway forwards the text to the LLM. When the LLM decides on a list of materials, the Gateway interprets this, queries the Catalogue API for the specific product IDs, and returns a unified JSON response to the UI. When Jim clicks "Checkout", the UI sends one request to the Gateway, which then communicates with the Shopping Cart API to add the items.
    

**(c) Drawbacks of Direct Communication (Replacing the diagram question)**

1. **Security Risks (Exposed Credentials):** Communicating directly with the external LLM requires API keys (which cost money per request). Storing these secret keys directly within the client-side code (mobile app or browser) is highly insecure, as malicious users could extract the keys and use them, incurring massive costs for the retail chain.
    
2. **Tight Coupling & High Client Complexity:** The frontend UI would need to understand the distinct network locations, authentication protocols, and data formats of three completely different services. This bloats the client code and means that any change to a backend API requires a mandatory update to the client application.
    
3. **Increased Latency (Network "Chattiness"):** The client device would have to make multiple round-trip network calls over the public internet (e.g., send prompt to LLM -> wait for response -> parse response -> send search query to Catalogue -> wait for response). An API Gateway handles this multi-step orchestration on the backend over a high-speed internal network, sending only a single, fully compiled response back to the client over the slower public internet connection.
    

**(d) Plugin Architecture for the LLM**

- To avoid vendor lock-in, the team can use the **Plugin** (or Adapter) pattern within the API Gateway. They define a common interface (e.g., `ILLMProvider` with a method `generateChatResponse()`).
    
- Different providers (OpenAI, local models) are written as separate plugin modules that implement this interface. The Gateway loads the active plugin via dependency injection or configuration files at runtime, allowing the LLM to be swapped out without altering the core orchestration logic.
**(d) Plugin Architecture for the LLM**

- To avoid vendor lock-in, the team can use the **Plugin** (or Adapter) pattern within the API Gateway. They define a common interface (e.g., `ILLMProvider` with a method `generateChatResponse()`).
    
- Different providers (OpenAI, local models) are written as separate plugin modules that implement this interface. The Gateway loads the active plugin via dependency injection or configuration files at runtime, allowing the LLM to be swapped out without altering the core orchestration logic.
    

---

### **4. Software Maintenance and Testing**

**(a) Bad Smells in the BDD Feature File**

1. **Smell:** Imperative script-like steps / Implementation details (e.g., `And I click checkout`).
    
    - _Why it's bad:_ It binds the test tightly to the UI. If the UI changes from a "Checkout" button to a "Pay Now" swipe, the test breaks even if the business logic is fine.
        
    - _Refactoring:_ Make it declarative. `When I proceed to payment`.
        
2. **Smell:** Duplication of Scenarios (Scenarios 1 and 2 are nearly identical except for the code and price).
    
    - _Why it's bad:_ If the baseline price of shipping or paint changes, you have to update multiple scenarios manually.
        
    - _Refactoring:_ Use a `Scenario Outline` with an `Examples` data table for the discount codes and expected totals.
        
3. **Smell:** Overly complex/rambling scenarios (Scenario 4 adds, removes, clears, and adds items back).
    
    - _Why it's bad:_ This scenario is testing the shopping cart's add/remove functionality _and_ the discount code simultaneously. If it fails, it's hard to tell which part broke.
        
    - _Refactoring:_ Split the test. Have one set of tests purely for adding/removing cart items, and a much simpler scenario testing the "FREEBRUSHWITHPAINT" logic.
        

**(b) The Fallacy of Code Coverage**

- 100% code coverage simply means that every line of code was executed during the test run. It **does not** guarantee that the code produces the correct results, because tests might be missing assertions. Furthermore, coverage cannot tell you if you failed to implement a required feature entirely, nor does it guarantee that complex edge cases (e.g., applying a discount code that makes the cart total negative) have been tested.
    

**(c) Mutation Testing Terminology**

- **(i) Mutation:** A deliberate, artificial defect (a small syntactic change) injected into the source code by an automated tool (e.g., changing `a + b` to `a - b`, or `>=` to `>`).
    
- **(ii) Mutant:** The new, altered version of the program that contains the single mutation.
    
- **(iii) Survivor:** A mutant that successfully passes all the tests in the test suite. This indicates a weakness in the test suite, as it failed to catch the injected bug.
    
- **(iv) Effectiveness:** A metric calculated by dividing the number of _killed_ mutants (where tests failed) by the total number of valid mutants. A higher percentage indicates a more effective, robust test suite.