# Assignment 2

## Problem statement.
In your first assignment, you brainstormed a set of problems that your project might address. You should now pick one of those problems and refine it for this assignment. Your problem statement should be less than one page in length, and should include the following deliverables:

- A problem domain, with a title and a few sentences explaining it. Remember that a domain is not a particular problem but an area of activity that you are involved or interested in. Your explanation should include what your connection is and why you care about it. (See examples in the first assignment.)

- A problem: one key problem faced by users within this domain, with a title and a few sentences explaining it. Remember that a problem must be a difficulty experienced in the problem domain by users; it should not be a flaw or limitation of existing solutions.

- A stakeholder list, with a name for each kind of stakeholder, and a sentence explaining their role (if any) in the problem.

- Evidence and comparables: a list of pieces of evidence that the problem is real, which will likely be citations of publicly available resources (but could also be observations that you have made about the extent or nature of the problem), and some comparables, being applications that address this or a related problem. Your list of evidence and comparables should have at least 5 entries, each with a title, an optional link, and a sentence or two explaining it.


## Application pitch. 
Construct a succinct but compelling pitch that explains, in terms understandable to a lay user, how your application will solve the problem. Your pitch should be around half a page in length, and should include the following deliverables:

- A name: pick a fun and memorable name for your application.

- A motivation: summarize the problem that your application will solve in a sentence.

- Key features: explain up to three features in a brief narrative, ensuring that each has a name and a simple explanation of what the feature is; why it helps mitigate the problem; and how it impacts stakeholders. (See the note in the advice section on concepts vs. features.)


## Concept design. 
Design a set of concepts that will embody the functionality of your app and deliver its features. We expect you to have 3-5 concepts. Fewer than 3 concepts would probably mean limited functionality or a lack of separation of concerns; more than 5 likely suggests overambition or lack of focus. (Talk to us if you think you need more!) The deliverables are:

- The concept specifications, written in the standard form.

- Some essential synchronizations. You do not need an exhaustive collection of synchronizations, but should capture (a) any essential design ideas that involve multiple concepts; (b) representative syncs for kinds of sync that are common throughout your application (such as syncs for access control or notification).

- A brief note, at most half a page long, explaining the role that your concepts play in the context of the app as a whole. For example, if you have an authentication or authorization concept, you should say how it’s used to control access to other particular concepts. You should also explain how generic type parameters will be instantiated whenever it’s non obvious. (For example, that a generic user type will be bound to the users of an authentication concept is obvious; that the targeted items of an upvoting concept are users would not be.)


## UI Sketches. 
Construct some low-fidelity sketches of your user interface that show the primary user interface elements and their rough layout, annotated with pointers or comments to explain anything that might not be obvious. You should omit any error handling and standard interactions (such as user registration), but should aim to convey how the essential features appear and are used. You can use any tool you like to draw the sketches, or you can draw them by hand and scan them. You should include them in your design document as images referenced in markdown.


## User journey. 
Write a narrative that follows a single stakeholder as they encounter the problem and use your designed app to address it. Tell their story in chronological order: what triggers their need, the steps they take with the app (referring to the wireframes), and the outcome. A good journey should both persuade a reader that the problem is worth solving, and illustrate how your app might help solve it. The deliverable here is a few short paragraphs that form a coherent story but identify individual steps clearly, and that reference your sketches when appropriate.