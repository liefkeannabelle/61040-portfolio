# PSET 1
**Exercise 1:** Reading a concept
1. Invariants. 

Q: What are two invariants of the state? (Hint: one is about aggregation/counts of items, and one relates requests and purchases). Say which one is more important and why; identify the action whose design is most affected by it, and say how it preserves it.

A: One invariant is that there is at most one request for a given item. A second invariant is that the number of purchases for an item is limited by the count of that item's request. I would say the second invariant is more important as it more greatly affects the users' real-life actions and prevents redundant purchases whereas the other invariant primarily avoids confusion. For this invariant, the *purchase* action is most impacted in that it must confirm the purchaser is purchasing less than or equal to the remaining requested amount and decrement this value accordingly.

2. Fixing an action. 

Q: Can you identify an action that potentially breaks this important invariant, and say how this might happen? How might this problem be fixed?

A: The *removeItem* action poses a potential threat to this invariant. By deleting an item, there is no longer a count value for the request, but existing purchases will still exist. If I were to delete an item from my registry after one of my friends had already purchased one for me, this invariant is no longer maintained. The simplest way to prevent this is to not allow items to me removed after the registry is marked as active as no purchases are made prior to activation. Alternatively, you could limit removal to items with no purchases. Since purchases cannot easily be "undone", one of these approaches is necessary to avoid this problem.

3. Inferring behavior.

Q: The operational principle describes the typical scenario in which the registry is opened and eventually closed. But a concept specification often allows other scenarios. By reading the specs of the concept actions, say whether a registry can be opened and closed repeatedly. What is a reason to allow this?

A:  Yes, with the given specification, a registry can be repeatedly opened and closed because the only condition for either action is that the regsitry is currently in the alternative state with no measure for how many times its state had been changed. While not described in the operational principle, I think it makes sense to allow it to be opened and closed more than once to allow for reuse of the same registry across time while allowing edits to happen behind the scenes. For example, if someone has a bridal shower, they may post a registry and guests will purchase gifts. After the event, they may temporarily close the registry to adjust certain requests regarding items they had not yet been gifted if their needs or opinions had changed but without having to start over completely. Then, leading up to the wedding, they may reopen the registry for wedding guests to purchase gifts. 

4. Registry deletion. 

Q: There is no action to delete a registry. Would this matter in practice?

A: Practically, there is no reason you would *need* to be able to delete a registry. A closed registry is, in practice, equivalent to a deleted registry in terms of what actions are available, with the exception of keeping the ability to re-open it. However, users who are creating registries may appreciate the ability to delete old registries for the purpose of keeping their experience relatively streamlined. However, within this concept itself, I do not think it is necessary.

5. Queries. 

Q: What are two common queries likely to be executed against the concept state? (Hint: one is executed by a registry owner, and one by a giver of a gift.)

A: The registry owner will likely be most interested in querying information regarding the set of purchases, particularly what items have already been purchased in some quantity. The gift givers will instead most likely be querying the requests, specifically for the items requested.

6. Hiding purchases. 

Q: A common feature of gift registries is to allow the recipient to choose not to see purchases so that an element of surprise is retained. How would you augment the concept specification to support this?

A: To allow for this, I would add a flag for *surpriseMode* as a part of the Registry that can be set by the registry owner. When this flag is set, that would affect the visiblity of purchases to the owner. 

7. Generic types. 

Q: The User and Item types are specified as generic parameters. The Item type might be populated by SKU codes, for example. Explain why this is preferable to representing items with their names, descriptions, prices, etc.

A: Using this generic Item type allows for more simple specification within the GiftRegistration concept and to use whatever Item specification best suits the specific use of this concept. This particularly simplifies the maintenance of the invariant regarding having no duplicate items on the registry.

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