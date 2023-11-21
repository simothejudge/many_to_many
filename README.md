## Deferred Acceptance Algorithm - Many to Many

#### Introduction

In mathematics, economics, and computer science, the Gale–Shapley algorithm (also known as the deferred acceptance algorithm or propose-and-reject algorithm) is an algorithm for finding a solution to the stable matching problem, named for David Gale and Lloyd Shapley. It takes polynomial time, and the time is linear in the size of the input to the algorithm. It is a truthful mechanism from the point of view of the proposing participants, for whom the solution will always be optimal.

![Gale-Shapley.gif](attachment:Gale-Shapley.gif)

In 1962, David Gale and Lloyd Shapley proved that, for any equal number of participants of each type, it is always possible to find a matching in which all pairs are stable.


The Gale–Shapley algorithm involves a number of "rounds" (or "iterations"). In terms of job applicants and employers, it can be expressed as follows:

In each round, any subset of the employers with open job positions makes a job offer to the applicant they prefer, among the ones they have not yet already made an offer to.
Each applicant who has received an offer evaluates it against their current position (if they have one). If the applicant is not yet employed, or if they receive an offer from an employer they like better than their current employer, they accept the new offer and become matched to the new employer (possibly leaving a previous employer with an open position). Otherwise, they reject the new offer.
This process is repeated until everyone is employed.

The algorithm guarantees that: 

**Everyone gets matched**

At the end, there cannot be an applicant and employer both unmatched. An employer left unmatched at the end of the process must have made an offer to all applicants. But an applicant who receives an offer remains employed for the rest of the process, so there can be no unemployed applicants. Since the numbers of applicants and job openings is equal, there can also be no open positions remaining.

**The matches are stable**

If an applicant X and an employer Y could form an unstable pair, Y would have made an offer to X prior to the offer made by Y to their actual match. But X would have accepted this offer and kept it over the offer they ended up with. Therefore, no such pair is possible.


### The proposed algorithm 
We took the theroy of Gale–Shapley and applied to a specific case: the many-to-many association problem. Strating from a simpler case of One-to-One problem, we developed a Many-to-Many solution, by considering each class having a certain capacity limit. Below you can see the different setting of the two problems.

#### Simple case: one to one algorithm
![M2M_phase123.png](attachment:M2M_phase123.png)

![M2M_phase456.png](attachment:M2M_phase456.png)

#### Adjusted: many to many algorithm
The main difference from the previous problem is that now Jobs and Users have both a capacity limit; in the example we can imagine that every candidate can have simoultaneously more than 1 job, but of course there is a limit to how many simoultaneous jobs one can have based on their availability. In the same way, every job can be choosen by multiple people dividing the work among themselves, but of course there is a maximum amount to respect. 


###### Phase 1:

we choose an arbitrary starting point (let's start by looking at the users and specifically following the order given): 
User x: 
    - User x likes Job 2 
    - Check: Job 2 has a free spot (no capacity reach)
    - Check: Job 2 likes User x back
    - User x and Job 2 matches
    - Update: the first spot of User x and Job 2 are filled reciprocally

User y: 
    - User y likes Job 1 
    - Check: Job 1 has a free spot (no capacity reach)
    - Check: Job 1 likes User x back
    - User y and Job 1 matches
    - Update: the first spots of User y and Job 1 are filled reciprocally

User z: 
    - User z likes Job 8 
    - Check: Job 8 has a free spot (no capacity reach)
    - Check: Job 8 likes User z back
    - User z and Job 8 matches
    - Update: the first spots of User z and Job 8 are filled reciprocally

User w: 
    - User w likes Job 2 
    - Check: Job 2 has a free spot (no capacity reach)
    - Check: Job 2 likes User w back
    - User w and Job 2 matches
    - Update: the spot of User w if filled with Job 2 and the second spot of Job 2 is filled with User w


###### Phase 2:
User x: 
    - User x likes Job 4 as second preference
    - Check: Job 4 has a free spot (no capacity reach)
    - Check: Job 4 likes User x back
    - User x and Job 4 matches
    - Update: the second spot of User x and Job 4 are filled reciprocally

User y: 
    - User y likes Job 2
    - Check: Job 2 has reach capacity: if Job 2 prefers User y to the User in the last spot (User w) then we substitute
    - Check: Job 2 likes User y back and it likes User y more than User w (second preference)
    - User y and Job 2 matches
    - Update: the second spot of User y is filled with Job 2 while the last spot of Job 2 is substituted with User y, leavng User w, deleting Job 2 from User w's list of recommendations.

User z: 
    - User z likes Job 7 
    - Check: Job 7 has a free spot (no capacity reach)
    - Check: Job 7 likes User z back
    - User z and Job 7 matches
    - Update: the second spot of User z and the first one of Job 7 are filled reciprocally

User w: 
    - User w likes Job 4
    - Check: Job 4 has a free spot (no capacity reach)
    - Check: Job 4 doesn't likes User w back
    - User w and Job 4 don't match, User w is left with no assignment for this phase

###### Phase 3:
User x: 
    - User x likes Job 3 as third preference
    - Check: Job 3 has a free spot (no capacity reach)
    - Check: Job 3 likes User x back
    - User x and Job 3 matches
    - Update: the third spot of User x and the first one of Job 3 are filled reciprocally

User y: 
    - User y has reached capacity. 

User z: 
    - User z likes Job 4 
    - Check: Job 4 has a free spot (no capacity reach)
    - Check: Job 4 likes User z back
    - User z and Job 4 matches
    - Update: the second spot of User z and of Job 4 are filled reciprocally

User w: 
    - User w likes Job 3
    - Check: Job 3 has a free spot (no capacity reach)
    - Check: Job 3 likes User w back
    - Update: the first spot of User w and the second one of Job 3 are filled reciprocally



This logic is applied in phase 4, 5, 6 and at the end we can see that everyone got matched (even though not for every job capacity was reached) and the matching is stable 



#### Imput:
----------------
- `list of users (users = list(User))` each one with its own list of preferred jobs (prefs = list(Job))
- `list of jobs (jobs = list(Job))` each one with its own list of preferred users (prefs = list(User))
- `users_capacities = (dict{user:M})` M variabile 
- `jobs_capacities = job's capacity (dict{job:N})` N variabile 
- `free_subusers` = list of the free_subusers which haven't been matched yet or have been rejected or switched
    (initialized to all sub_users)
- `match = dict{job:[users]}` (empty)

#### Additional imputs to define during the algorithm: 
----------------
- `submatch = dict{subuser: subjob}`, resetting every round and every round it is rebuilt
- `counter = dict{user : count_match }`, counting every time a user is being matched to different jobs, it resets every round

#### PseudoCode: 
-----------------

    while ∃ free_subusers:

        subuser = first subuser from the free_subusers list 
        user = parent of subuser
        favourite = favourite job from user preferences
        
        if user is in favourite's preferences:
           add the current match (job -> user) to the temporary match list of the favurite job 
           temp_match <- ordered_list(job_prefs) ∩ list(temp_match) (ordered based on job_pref) 
           temp_match <- temp_match[:N] where N is the capacity of the job favourite
           
           if the user is no more in temp_match: 
               reappend the subuser to the free_subusers (no match happened)
            
            match[favourite] = temp_match
            initialize submatch
            initialize the counter to 0 for each key_user
            
            for each job_key in the updated match dictionary: 
                for i (sub_job index) and user_value in match[job_key]:
                    j = counter of how many times the user appears in a match (sub_user index)
                    update the submatch dictionary where: {job_key_i : user_value_j}
                    
            
            if there is any sub_users left out from the previous submatch: 
                add the switched (loser) subuser to the free_subusers list
            
            save the current submatch for the next round confrontation 
           
        else if the user is not in its favourite job's preferences:
            add again the initial subuser at the end of the list of free_subusers

        remove the favourite job from user's preferences
        
        if there are no preferences left to be proposed to:
            get all the subusers of the current user and remove them from the free_subusers list 
