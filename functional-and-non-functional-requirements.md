# Functional and Non-Functional Requirements

Before jumping into designing the system’s architecture or writing code, it’s important to first understand **what** the system needs to do and **how** it should perform. This means getting clear on the system’s **requirements**.

Think of it like building a house: before you start construction, you need to decide what rooms and spaces the house should have (like bedrooms, kitchen, and bathrooms), and also what kind of living experience you want (should it be well-lit, energy efficient, secure, or easy to maintain). Similarly, in system design, you must know:

* **What features or functions the system should have** (the rooms and spaces in the house)
* **What qualities or standards the system should meet** (the comfort, safety, and efficiency of the house)

These are called **functional** and **non-functional requirements**.

## Functional Requirements: What the System Should Do

Functional requirements describe the specific features and actions the system must provide to users. They answer the question: _“What should the system do?”_

**For example:**

* Allow users to sign up and log in.
* Let users add items to a shopping cart.
* Send notifications when a message arrives.

These requirements focus on user interactions and business rules - the core capabilities your system must deliver.

## Non-Functional Requirements: How the System Should Perform

Non-functional requirements describe the qualities and constraints of the system. They answer the question: _“How well should the system do it?”_

**For example:**

* The system should load pages quickly (performance).
* It should be secure and protect user data (security).
* It must handle millions of users at the same time (scalability).
* The system should be available 99.99% of the time (reliability).

These requirements ensure your system is not just functional but also fast, reliable, and secure.

### Why Both Matter

* **Functional requirements** make your system useful.
* **Non-functional requirements** make your system good to use.

Skipping either can lead to problems: a system might have all features but be too slow or crash often, or it might be fast but lack important functions.

### Quick Summary

| Type           | Focus                        | Example                                      |
| -------------- | ---------------------------- | -------------------------------------------- |
| Functional     | What the system does         | User can reset password via email link.      |
| Non-Functional | How well the system performs | System responds to requests within 1 second. |

With a clear understanding of these requirements, you’re ready to start designing systems that not only work but work well. Next, we’ll explore how to identify and gather these requirements for any project.
