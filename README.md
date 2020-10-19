# heirloom
State and data management library for ClojureScript with optional buildings for React, Firebase and other popular solutions. 

**ALL OF THE BELOW IS A WORK IN PROGRESS**

# Motivation
There are many different solutions to manage local state: re-frame, redux, flux and derivatives. There are many ways to fetch data: REST, graphQL, Pathom, falcor, Relay, etc. There are many ways to draw all above this in a browser (though React is a heavy favorite for ClojureScript). There is Fulcro that gives you a framework with tons of opinions on how to do everything together. There are many amazing technologies to store data for many specific use cases. Serverless approach is red hot today with a framework for that popping out practically every week. 

Making sense of all of that is challenging enough. Trying to tie everything together is even harder. The worst part that is that there is no agreed pattern on how to build front-end applications: MVC, MVP, MVVM, and similar abbreviations treated by every library or framework differently and requires developers to learn new patterns with every new library added. 

This library is an attempt to build a general approach on how to manage state, data, logic and UI on front-end without specific bindings to a specific technologies. It is highly experimental and this is me trying to reduce more then a decade of experience and frustration with front-end technologies into something simple and beautiful as MVC. For now, it will only target ClojureScript and possibly Cloud functions on node.js. 

## Basic Principles

1. Front-end state for most applications is just a partial local cache of some backend database. Very few libraries attempt to reflect this fact. Data & fetching should be a first-class citizen and data should drive most of the logic, not the other way around. 
2. Graph representation is a great way to think about data and models in a domain. It makes a perfect sense to store and operate with this structure everywhere except for edges (databases) where storage needs to be optimized for queries. 
3. Queries for data must be declarative and ideally easily composable.
4. There must be a clear separation between model (all domain structures and logic for those structures) and controllers (a thin layer that just calls model methods in a proper order, handles errors, etc.) 
5. Derivatives is a simple way for front-end to shape data for use in views. Those should be computed automatically based on source data and updated when world changes. 
6. Views should reflect a latest state of the world and update when stage changes automatically. 
7. Logic, views, data fetching should stay as declarative as possible and side-effects should be processed by a small batch of functions based on a state. 

# State 
Code for this library still lives in a few different projects I'm developing, but getting closer and closer to the place where I'm ready to fix the API. 

# Core structures 

This section will be updated with more details as things develop more. 

1. *Domain Graph* — a directed graph description of all the models that are present in the application. Contains nodes, optional schemas, and links between the nodes. 
2. *Domain Node* – a description of data structure or derivative from data structure. Explains data and methods for that data structure. Methods are simple functions that can be composed and reused for more than one node. Data also should be represented by standard data structures. 
3. *Graph Queries* — a declarative way to explain what data is required for an operation. Most likely based on EQL with slight changes to allow multiple parameters. 
4. *Query Parser* — translate a graph query to an ordered set of effects that can be processed by model to deliver data to request. Pathom and EQL inspired.
5. *State Graph* — graph that maintains a current local state of things. 
6. *Subscriptions* — a way to subscribe to changes inside the graph in a view or in any other logic piece. 
7. *Flows* — a core controller structure. Way to split logic into a set of functions that concern about one thing, join them together and control the flow of execution that allows mixing async and sync functions. 
8. *Dispatch* – a way to trigger changes. Similar to many other dispatches in libraries like re-frame, redux, etc, but with one twist: it must be issued towards a *domain node(s)*. This slight constraint allows to know exactly which nodes are "dirty" meaning some effects must be applied on them and avoid any value comparisons in state. Updates will be issued only on dirty nodes and their derivatives allowing to be very efficient with updates and bulking them together. 
9. *Effect handlers* — a set of functions that apply changes on a graph and trigger side-effects, queries, etc. 
