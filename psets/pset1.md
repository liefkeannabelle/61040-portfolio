# PSET 1
**Exercise 1:** Reading a concept
1. Invariants. 

Q: What are two invariants of the state? (Hint: one is about aggregation/counts of items, and one relates requests and purchases). Say which one is more important and why; identify the action whose design is most affected by it, and say how it preserves it.

A:

2. Fixing an action. 

Q: Can you identify an action that potentially breaks this important invariant, and say how this might happen? How might this problem be fixed?

A:

3. Inferring behavior.

Q: The operational principle describes the typical scenario in which the registry is opened and eventually closed. But a concept specification often allows other scenarios. By reading the specs of the concept actions, say whether a registry can be opened and closed repeatedly. What is a reason to allow this?

A:

4. Registry deletion. 

Q: There is no action to delete a registry. Would this matter in practice?

A:

5. Queries. 

Q: What are two common queries likely to be executed against the concept state? (Hint: one is executed by a registry owner, and one by a giver of a gift.)

A:

6. Hiding purchases. 

Q: A common feature of gift registries is to allow the recipient to choose not to see purchases so that an element of surprise is retained. How would you augment the concept specification to support this?

A:

7. Generic types. 

Q: The User and Item types are specified as generic parameters. The Item type might be populated by SKU codes, for example. Explain why this is preferable to representing items with their names, descriptions, prices, etc.

A:

**Exercise 2:** Extending a familiar concept
1. Complete the definition of the concept state.
- A: 
2. Write a requires/effects specification for each of the two actions. (Hints: The register action creates and returns a new user. The authenticate action is primarily a guard, and doesn’t mutate the state.)
- A:
3. What essential invariant must hold on the state? How is it preserved?
- A:
4. One widely used extension of this concept requires that registration be confirmed by email. Extend the concept to include this functionality. (Hints: you should add (1) an extra result variable to the register action that returns a secret token that (via a sync) will be emailed to the user; (2) a new confirm action that takes a username and a secret token and completes the registration; (3) whatever additional state is needed to support this behavior.)
- A:

**Exercise 3:** Comparing concepts

Deliverables: a concept specification for PersonalAccessToken and a succinct note about how it differs from PasswordAuthentication and how you might change the GitHub documentation to explain this.

**Exercise 4:** Defining familiar concepts

Deliverables: three concept specifications, with any subtleties explained in brief additional notes.

1. 

2. 

3. 