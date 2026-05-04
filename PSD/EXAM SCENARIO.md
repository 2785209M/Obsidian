You have joined a software team within a large retail chain that sells a variety of supplies of materials and tools that can be used for Do It Yourself (DIY) projects. Examples of projects include putting up a new shelf, decorating a bedroom with paint or wallpaper, refitting an entire bathroom and building a wooden out-building such as a shed.

The chain has an existing online shopping platform, which includes an acceptance test suite written using a behaviour driven development framework. An example feature file from the suite covering some functions of the shopping cart component is shown below.

```
Feature: Configure travel preferences
  Scenario: 
    Given an empty shopping cart
      And shipping costs 5 GBP
     When I apply the "TEN%0FF" discount code
      And I add a pot of paint worth 10 GBP
      And I click checkout
     Then the price to pay is 13.50 GBP

  Scenario: 
    Given an empty shopping cart
      And shipping costs 5 GBP
     When I apply the "TWENTY%0FF" discount code
      And I add a pot of paint worth 10 GBP
      And I click checkout
     Then the price to pay is 12 GBP

  Scenario: 
    Given an empty shopping cart
      And shipping costs 5 GBP and I apply the "FREESHIPPING" discount code
     When I add a pot of paint worth 10 GBP
      And I click checkout
     Then the price to pay is 10 GBP

  Scenario: 
    Given an empty shopping cart
      And shipping costs 5 GBP
     When I apply the "FREEBRUSHWITHPAINT" discount code
      And I add a roller worth 11 GBP
      And I add a pot of paint worth 10 GBP
      And I add masking tape worth 8 GBP
      And I clear the shoppping cart
      And I add a paint brush worth 4 GBP
      And I add a pot of paint worth 10 back in to my shopping cart
      And I click checkout
     Then the price to is 15 GBP
```

A popular feature of the platform allows customers to browse a catalogue of home improvement projects. Each project has instructions for completing the work and a list of the necessary materials and tools. While viewing a project, customers can click a button that adds the materials needed to their online shopping cart. For example, putting up a shelf requires a plank of wood, triangular brackets, screws and dry-wall plugs. The customer can adjust the shopping cart as normal. For example, they may want to remove supplies they already have. Customers can also add any required tools listed on the project catalogue page. Once the customer is satisfied with the order they can proceed through the online checkout as normal.

The software team see an opportunity to increase sales by making DIY projects easier to plan. The new app will allow customers to create a shopping list of supplies for a custom project using a Large Language Model (LLM). They have created the following user journey to describe their vision for "Chat-DI-AI":

> Jim, a customer, opens a prompt in Chat-DI-AI. He first lists the tools that he already owns (paintbrushes and a screwdriver). He states that he wants to redecorate his son's bedroom and for it not to cost more than £200. Chat-DI-AI responds by providing an overview of the steps and asking Jim for the room measurements. Jim provides these details and the chatbot asks what colour Jim is looking for. He wants to paint three walls in a mid-blue he's already picked, and a "feature wall" in very dark navy blue (his son is interested in space) but isn't sure of the shade. Chat-DI-AI suggests three colours for the feature wall, showing samples in the response against the mid-blue. Jim picks one. Chat-DI-AI asks about the ceiling and Jim states he wants to paint it white. Jim then asks if paint for the skirting board, door frame and door should be added. Chat-DI-AI apologises and asks what colour they should be. Jim replies white-satin. Jim then clicks a button to generate a shopping list. He notices that Chat-DI-AI has included masking tape with an explanation that this is needed for neat edges, but he removes this as he already has some. Chat-DI-AI has added a paint roller and tray to the shopping cart from the tools list, but omits paint brushes. Jim proceeds to the checkout. Chat-DI-AI sends Jim instructions for the project to his email address.

The team consists of three developers, Jane, John and Jo. The team follow an agile software development process and work in two week sprints. You join the team in the planning meeting for their second sprint. The first sprint appears to have gone well. The team used mob-programming to complete issue #1, creating a chat user interface (UI) and connecting it to the LLM. The team demonstrates the UI during the review meeting by creating a simple project for decorating a room, as described in the user journey. The marketing manager, Jess, expresses excitement at the prospect of trying it out for building the garden shed she has planned.

Jess is eager for an initial version of the product to be completed quickly, so the team agree to connect the LLM and UI to the product catalogue in the second sprint, so that they can be added to the customer's shopping cart. To be more efficient, the team decide to work separately during this sprint. They therefore create the following tasks in their issue tracker:

|ID|Title|Description|Estimate (weeks)|Priority|Assign-ee|
|---|---|---|---|---|---|
|2|Link LLM to catalogue|use catalogue API for this?|2 weeks|Must|Jane|
|3|Improve UI Look and Feel|Jess thought it a bit ugly at the moment.|2 weeks|Must|John|
|4|Add to shopping cart feature|need to check the shopping app REST API|1.5 weeks|Must|Jo|
|5|Remove un-needed materials feature|n/a|0.5 weeks|Must|Jo|
The senior developer, Jane, applies estimates to the features based on her judgement as to their relative complexity. Linking the LLM to the catalogue is felt to be about as difficult as improving the UI look and feel, for example. The team use the MoSCoW priority categories, with "Must" meaning the issue is to be completed in the next sprint.