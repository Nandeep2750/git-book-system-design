# HLD & LLD

System design is built on two main pillars:&#x20;

* **High-Level Design (HLD)** - the _big picture view_
* **Low-Level Design (LLD)** - the _detailed view_

Understanding both is key to designing any large-scale software system.

## **High-Level Design (HLD): The Big Picture**

### **What is HLD?**

High-Level Design is like looking at a map of a city before you start building houses. It gives you a bird’s-eye view of the whole system, showing the main parts (like databases, servers, APIs) and how they connect. HLD answers questions like:

* What are the main components of the system?
* How do these components talk to each other?
* What technologies or frameworks will we use?

### **Key Features of HLD**

* Focuses on overall system architecture and major modules.
* Uses diagrams to show how components interact (e.g., network diagrams, architecture diagrams).
* Helps everyone (developers, managers, clients) understand the system at a glance.
* Does not go into coding or detailed logic-just the structure.

### **Example**

Imagine designing an online shopping website.

* HLD would show:
  * Web servers for handling user requests
  * A database for storing products and orders
  * A payment gateway for transactions
  * How these parts connect (e.g., web server talks to the database, payment service talks to the bank API).

### **Benefits**

* Reduces risk by spotting problems early.
* Improves communication among team members.
* Make sure everyone shares the same vision before coding starts.

## **Low-Level Design (LLD): The Details**

### **What is LLD?**

Low-Level Design is like creating the blueprints for each house in the city. It zooms in on each part shown in the HLD and describes exactly how it works. LLD answers questions like:

* What classes and functions will we write?
* What will the database tables look like?
* How will we handle errors or edge cases?

### **Key Features of LLD**

* Focuses on the internal logic of each component.
* Includes class diagrams, data structures, algorithms, and database schemas.
* Provides step-by-step plans for developers to follow during coding.
* Ensures all requirements are met with precise technical details.

### **Example**

Continuing the online shopping website:

* LLD would show:
  * The exact structure of the “User” and “Order” tables in the database
  * The API endpoints for “add to cart” or “checkout”
  * How the payment logic handles errors (like failed transactions).

## **HLD vs. LLD: Quick Comparison**

| Aspect       | High-Level Design (HLD)             | Low-Level Design (LLD)                 |
| ------------ | ----------------------------------- | -------------------------------------- |
| Focus        | Overall system structure            | Detailed implementation                |
| Artifacts    | Architecture diagrams, data flow    | Class diagrams, algorithms, schemas    |
| Audience     | All stakeholders                    | Developers, technical team             |
| Example      | Web server ↔ Database ↔ Payment API | User table schema, payment error logic |
| When Created | Early in the project                | After HLD, before coding               |

## **Summary**

* **HLD** gives you the big picture: what the system is and how its main parts connect.
* **LLD** dives into the details: how each part is built and how it works internally.
* Both are essential- HLD sets the vision, and LLD makes it real.

### **Next Steps**

Now that you know the difference, you’re ready to explore how to create HLDs and LLDs for real-world systems, starting with functional and non-functional requirements!
