# PSET 2
## Concept Questions
1. Contexts are used to keep generated shortened URLs unique within a certain scope. In use within the URL shortening app, the contexts are the shortUrlBases. Each base must have all unique suffixes so that no two shortened URLs are equivalent.

2. In order to successfully maintain uniqueness of URLs within each context, NonceGeneration must store some information regarding what strings have already been used. In the described implementation, there would need to be some deterministic way to convert from an number to a string, such that knowing the last used number indicates what the next available string is. 

3. Pro: these strings are easy to use and share since they consist of words individuals know, rather than strings of random characters. Con: the generated strings, while shortened, end up longer than if you could use any sequence of characters to avoid the heightened probability of collision. To capture this in the concept design for NonceGeneration, I would modify as follows:
```
  concept NonceGeneration [Context]
  purpose generate unique strings within a context
  principle each generate returns a string not returned before for that context
  state
    a set of Contexts with
      a used set of Strings
      a wordBank set of Strings
  actions
    generate (context: Context) : (nonce: String)
      requires there are available strings in wordBank that are not yet in used  
      effect returns a nonce from wordBank that is not already in used
```

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