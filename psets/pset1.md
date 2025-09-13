# PSET 1
## Exercise 1
**Reading a concept**
1. Invariants. 

One invariant is that there is at most one request for a given item. A second invariant is that the number of purchases for an item is limited by the count of that item's request. I would say the second invariant is more important as it more greatly affects the users' real-life actions and prevents redundant purchases whereas the other invariant primarily avoids confusion. For this invariant, the *purchase* action is most impacted in that it must confirm the purchaser is purchasing less than or equal to the remaining requested amount and decrement this value accordingly.

2. Fixing an action. 

The *removeItem* action poses a potential threat to this invariant. By deleting an item, there is no longer a count value for the request, but existing purchases will still exist. If I were to delete an item from my registry after one of my friends had already purchased one for me, this invariant is no longer maintained. The simplest way to prevent this is to not allow items to me removed after the registry is marked as active as no purchases are made prior to activation. Alternatively, you could limit removal to items with no purchases. Since purchases cannot easily be "undone", one of these approaches is necessary to avoid this problem.

3. Inferring behavior.

Yes, with the given specification, a registry can be repeatedly opened and closed because the only condition for either action is that the regsitry is currently in the alternative state with no measure for how many times its state had been changed. While not described in the operational principle, I think it makes sense to allow it to be opened and closed more than once to allow for reuse of the same registry across time while allowing edits to happen behind the scenes. For example, if someone has a bridal shower, they may post a registry and guests will purchase gifts. After the event, they may temporarily close the registry to adjust certain requests regarding items they had not yet been gifted if their needs or opinions had changed but without having to start over completely. Then, leading up to the wedding, they may reopen the registry for wedding guests to purchase gifts. 

4. Registry deletion. 

Practically, there is no reason you would *need* to be able to delete a registry. A closed registry is, in practice, equivalent to a deleted registry in terms of what actions are available, with the exception of keeping the ability to re-open it. However, users who are creating registries may appreciate the ability to delete old registries for the purpose of keeping their experience relatively streamlined. However, within this concept itself, I do not think it is necessary.

5. Queries. 

The registry owner will likely be most interested in querying information regarding the set of purchases, particularly what items have already been purchased in some quantity. The gift givers will instead most likely be querying the requests, specifically for the items requested.

6. Hiding purchases. 

To allow for this, I would add a flag for *surpriseMode* as a part of the Registry that can be set by the registry owner. When this flag is set, that would affect the visiblity of purchases to the owner. 

7. Generic types. 

Using this generic Item type allows for more simple specification within the GiftRegistration concept and to use whatever Item specification best suits the specific use of this concept. This particularly simplifies the maintenance of the invariant regarding having no duplicate items on the registry.

## Exercise 2
**Extending a familiar concept**
1. Complete the definition of the concept state.
```
a set of Users with
    a username String
    a password String
```
2. Write a requires/effects specification for each of the two actions. (Hints: The register action creates and returns a new user. The authenticate action is primarily a guard, and doesn’t mutate the state.)
```
register (username: String, password: String): (user: User)
    requires: there is not already a User with the given username
    effects: creates a new User with the given username and password, adds it to the set of Users, and returns it

authenticate (username: String, password: String): (user: User)
    requires: a User with the given username exists and its password matches the given password
    effects: returns the corresponding User
```
3. What essential invariant must hold on the state? How is it preserved? <br>
Each User must have a unique username. This is maintained by the requirement on *register* that the username is not already taken by an existing User.

4. One widely used extension of this concept requires that registration be confirmed by email. Extend the concept to include this functionality. (Hints: you should add (1) an extra result variable to the register action that returns a secret token that (via a sync) will be emailed to the user; (2) a new confirm action that takes a username and a secret token and completes the registration; (3) whatever additional state is needed to support this behavior.)

For this extension, the concept design would look like this:
```
concept PasswordAuthentication
purpose limit access to known users
principle after a user registers with a username and a password, they can authenticate with that same 
          username and password and be treated each time as the same user
state
    a set of Users with
        a username String
        a password String
        a confirmed Flag
        a token Token
actions
    register (username: String, password: String): (user: User, token: Token)
        requires: there is not already a User with the given username
        effects: generates a token for the User; creates a new User with the given username and password,  
                 confirmed flag set to false, and generated token; adds the new User to the set of Users; 
                 returns the new User
    authenticate (username: String, password: String): (user: User)
        requires: a User with the given username exists, its password matches the given password, and its 
                  confirmed flag is set to true
        effects: returns the corresponding User
    confirm (username: String, token: Token): (user: User)
        requires: a User with the given username exists in the set of Users, its confirmed flag is false, and 
                  the input token matches its token
        effects: updates the indicated User's confirmed flag to true and returns the User
```
## Exercise 3
**Comparing concepts**
```
concept PersonalAccessToken
purpose authenticate and validate permissions
principle after a user generates a token, it is used to gain access to resources the user has access to, including personal and organization repositories, and can perform authorized actions on these resources.
state
    a set of Tokens with
        a user User
        a token_string String
        a scope set Scopes
        a start_time Time
        an active Flag
actions
    generate (user : User, scopes : Scopes): Token
        requires: valid user with specified scope
        effect: generates and returns a token for the given user with the given scope
    deactivate (token : Token): Token
        requires: token is currently active
        effect: active flag set to false and token returned
    use (token : Token, scope: Scope)
        requires: token includes desired scope of usage
        effect: desired action is validated by token
```

PersonalAccessToken differs from PasswordAuthentication because a user can have multiple tokens with different scopes, but a single password that serves a single universal purpose. However, in the current GitHub explanation, this distinction is unclear and causes confusion while reading. This could be better specified in early paragraphs to justify the choice of using a token rather than a password.

## Exercise 4
**Defining familiar concepts**

Deliverables: three concept specifications, with any subtleties explained in brief additional notes.

1. Billable Hours Tracking
```
concept BillableHours
purpose allow employees to track their billable hours by task
principle when beginning a task, a worker will start the timekeeper with a note of what work is being done. upon completion, the employee will end the timekeeper for a record of how much time was spent on the task.
state
    a set of Sessions with
        a session_id Number
        a user User
        a start_time Time
        a duration Number
        a description String
        an ongoing Flag
actions
    start_session (user : User, start_time : Time, description : String) : Session
        requires: the user is not associated with an ongoing task
        effects: a new Session is created with the given user, start_time, and description, the duration set to 0,
        and the ongoing flag set to true; the created session is returned
    end_session (session_id : Number, user : User, end_time : Time) : Session
        requires: the user is the user associated with the session of the provided id
        effects: for the given session, the duration is set to the difference between the given end_time and the
        session's start_time and the ongoing flag is set to false; the updated session is returned
    edit_duration (session_id : Number, user : User, duration : Number) : Session
        requires: the user is the user associated with the session of the provided id
        effects: the session's duration is updated to the provided duration value
```
Additional considerations:
- For editing the duration, it could be enforced that times could only be manually decreased, as there isn't a compelling case for why you would need to manually increase the billed time.
- For editing the duration, it could also be introduced that an additional user (i.e. manager, supervisor, etc) needs to approve the request before the session is updated.

2. Conference Room Booking
```
concept ConferenceRoomBooking
purpose allow for room booking with an organization
principle users within the organization can reserve a room within the organization. users can also delete their reservations as needed. 
state
    a set of Rooms with 
        a room_name String
        a set of Reservations with
            a reservation_id Number
            a user User
            a start_time Time
            a duration Number
            a title String
actions
    reserve (user : User, room_name : String, start_time : Time, duration : Number, title : String) : Reservation
        requires: the room is available for the entire duration starting at the start_time
        effects: a reservation is created with the provided information and added to the set of reservations for the requested room
    delete_reservation (reservation_id : Number, user : User)
        requires: the user is the user associated with the given reservation
        effects: the reservation is removed from the set of reservations for that room
```
3. Electronic Boarding Pass
```
concept BoardingPass
purpose provide a digital boarding pass 
principle after checking in, passengers can add a digital version of their boarding pass to the digital wallet. the airline can keep the information on this pass up-to-date and use it during boarding.
state
    a set of Flights with
        a flight_id Number
        a flight_status Status
        a departure_time Time
        an assigned_gate String
        a set of Seats with
            a seat_name String
            an assigned Flag
            an optional passenger User
    
    a set of Passes with
        a passenger User
        a flight Flight
        a seat Seat

actions
    create_pass (passenger : User, flight : Flight) : Pass
        requires: the passenger is the in the flight's Seat set
        effects: a pass is created with the given passenger and flight
    update_gate (flight_id : Number, new_gate : String) : Flight
        requires:
        effects: the assigned_gate for the given flight is updated to new_gate and the flight is returned
    update_departure (flight_id : Number, new_departure : Time) : Flight
        requires:
        effects: the departure_time for the given flight is updated to new_departure and the flight is returned
    update_flight_status (flight_id : Number, new_status : Status) : Flight
        requires:
        effects: the flight_status for the given flight is updated to the new_status and the flight is returned 
    swap_seat (flight_id : Number, old_seat: Seat, new_seat : Seat)
        requires: 
        effects: the passengers in the old_seat and new_seat are swapped (including if either seat was originally unassigned) and their assigned flags are adjusted to reflect this change
```