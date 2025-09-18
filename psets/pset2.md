# PSET 2
## Concept Questions
1. Contexts. The NonceGeneration concept ensures that the short strings it generates will be unique and not result in conflicts. What are the contexts for, and what will a context end up being in the URL shortening app?

2. Storing used strings. Why must the NonceGeneration store sets of used strings? One simple way to implement the NonceGeneration is to maintain a counter for each context and increment it every time the generate action is called. In this case, how is the set of used strings in the specification related to the counter in the implementation? (In abstract data type lingo, this is asking you to describe an abstraction function.)

3. Words as nonces. One option for nonce generation is to use common dictionary words (in the style of yellkey.com, for example) resulting in more easily remembered shortenings. What is one advantage and one disadvantage of this scheme, both from the perspective of the user? How would you modify the NonceGeneration concept to realize this idea?

## Synchronization Questions
1. Partial matching. In the first sync (called generate), the Request.shortenUrl action in the when clause includes the shortUrlBase argument but not the targetUrl argument. In the second sync (called register) both appear. Why is this?

2. Omitting names. The convention that allows names to be omitted when argument or result names are the same as their variable names is convenient and allows for a more succinct specification. Why isn’t this convention used in every case?

3. Inclusion of request. Why is the request action included in the first two syncs but not the third one?

4. Fixed domain. Suppose the application did not support alternative domain names, and always used a fixed one such as “bit.ly.” How would you change the synchronizations to implement this?

5. Adding a sync. These synchronizations are not complete; in particular, they don’t do anything when a resource expires. Write a sync for this case, using appropriate actions from the ExpiringResource and URLShortening concepts.

## Extending the Design
1. Design a couple of additional concepts to realize this extension, and write them out in full (but including only the essential actions and state). It should not be necessary to make any changes to the existing concepts.

2. Specify three essential synchronizations with your new concepts: one that happens when shortenings are created; one when shortenings are translated to targets; and one when a user examines analytics.

3. As a way to assess the modularity of your solution, consider each of the following feature requests, to be included along with analytics. For each one, outline how it might be realized (eg, by changing or adding a concept or a sync), or argue that the feature would be undesirable and should not be included:

- Allowing users to choose their own short URLs;
- Using the “word as nonce” strategy to generate more memorable short URLs;
- Including the target URL in analytics, so that lookups of different short URLs can be grouped together when they refer to the same target URL;
- Generate short URLs that are not easily guessed;
- Supporting reporting of analytics to creators of short URLs who have not registered as user.