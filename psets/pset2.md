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
1. In presenting these syncs, these clauses are included to match the use case. Since targetUrl is not used to generate a nonce, its value is irrelavant to the invocation and can be omitted. In the second sync, the value of targetUrl and shortUrlBase are both used in register, so both are included in the invocation.

2. Like mentioned in question 1, not all clauses are included in every invocation, so it is important that it remains clear what parameters *are* being included. To maintain this clarity, the argument name and bound variable name must both be included if they differ. If this were not the case, there is room for multiple interpretations regarding what invocation is being made.

3. The first two syncs are both triggered at least in part by user input, thus include a request. However, the third sync is triggered internally upon completion of the registration, so it does not include a request.

4. The domain is equivalent to the context, which in this case is the shortUrlBase. So, to fix the domain, we want to hardcode the shortUrlBase, rather than passing in a variable value. To do this, the context parameter would be passed in as "bit.ly", rather than shortUrlBase, which is no longer needed.

5. To react accordingly to a shortUrl expiring, we would want to add the following sync:
```
sync expire
when ExpiringResource.expireResource() : resource
then UrlShortening.delete(shortUrl: resource)
```


## Extending the Design
1. We will define two additional concepts to make the desired behavior possible:
```
concept VisitAnalytics[Resource]
purpose maintain visit count for given resource
principle after creating a visitable resource, its creator can check to see how many visits it has received.
state 
  a set of Analytics with
    a resource Resource
    a visitCount Number 
actions
  addVisit(resource : Resource) : Resource
    requires: resource is in Analytics
    effects: increments visitCount and returns updated resource
  query(resource : Resource ) : Number
    requires: resource is in Analytics
    effects: returns the value of visitCount for resource


concept ResourceOwnership[Resource, User]
purpose associate an owner with a resource
principle upon creation of a resource, an owner is assigned such that certain privileges can be moderated.
state
  a set of Relations with
    a resource Resource
    an owner User
actions
  assignOwner(resource : Resource, owner : User) : Resource
    requires: resource is a valid Resource not already in Relations
    effects: resource is added to Relations with owner as its owner
  checkOwner(resource : Resource, possOwner : User) : Boolean
    requires: resource is in Relations
    effects: returns True if possOwner = owner for resource, otherwise returns False
```

2. Specify three essential synchronizations with your new concepts: one that happens when shortenings are created; one when shortenings are translated to targets; and one when a user examines analytics.
```
sync assignOwnership
when 
  UrlShortening.register(): shortUrl
  Request.getUser(): requestUser
then ResourceOwnership.assignOwner(resource : shortUrl, owner : requestUser)

sync recordLookup
when UrlShortening.lookup(shortUrl): targetUrl
then UrlAnalytics.addVisit(shortUrl)

sync showAnalytics
when 
  Request.analytics(shortUrl)
  Request.getUser(): requestUser
  ResourceOwnership.checkOwner(shortUrl, requestUser): True
then 
  UrlAnalytics.query(shortUrl): (records)
```

3. Here is my outline for each of the proposed additions:
- Allowing users to choose their own short URLs → This could be realized by allowing user input in selecting shortUrlSuffix. This would require adding an action to UrlShortening with a requirement for uniqueness like in NonceGeneration. There would not be a need to add to the state or syncs.
- Using the “word as nonce” strategy to generate more memorable short URLs → This could be realized by adding an alternate action to NonceGeneration for generation of nonce made of words. This would be triggered by a sync that, when given a request for a shortened URL of words, calls this new action.
- Including the target URL in analytics, so that lookups of different short URLs can be grouped together when they refer to the same target URL → This is not an extension that makes sense to implement in this way. While short URLs are bound to original URLs, they may each have different owners, and this information is privileged to said owner. Additionally, aggregation does not paint a picture that cannot be gathered from existing querying assuming an owner has multiple short URLs for the same target. Still, since these shortened URLs likely serve different purposes, it is best to keep the relationship as it stands now.
- Generate short URLs that are not easily guessed → Similar to the second augmentation, this could be achieved by adding another action in NonceGeneration for more complex, harder to guess nonces and a synce that allows a user to request this be used.
- Supporting reporting of analytics to creators of short URLs who have not registered as user → Given the generic type used to define User in my concept design, this can be achieved so long as there is some way to authenticate (e.g. IP address, etc). 