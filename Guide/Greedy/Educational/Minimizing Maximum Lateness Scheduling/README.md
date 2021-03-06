# Minimize Maximum Lateness Scheduling

This problem is discussed in [Dr. Gelfond Applied Algorithms](http://redwood.cs.ttu.edu/~mgelfond/FALL-2012/slides.pdf)

Category: Greedy

Difficulty: N/A

## Problem
### Overview
Given a set _R_ of requests where each request _i_ has a duration _t<sub>i</sub>_ and a deadline
_d<sub>i</sub>_, and a single unlimited resource, schedule each request _i_ into an interval with
a start time _s(i)_ and a finish time _f(i)_ i.e. _[s(i), f(i))_ with length _t<sub>i</sub>_ such
that each interval is disjoint.

A request _i_ is late if _f(i) > d<sub>i</sub>_. A requests lateness _L<sub>i</sub>_ is calculated by
_f(i) - d<sub>i</sub>_ if _i_ is late and 0 otherwise.

Find a schedule of all requests which:
- Starts at a given point _s_ and,
- Minimizes maximum lateness, L = max<sub>i</sub>(L<sub>i</sub>)


### Input Format
- The set R of requests, each having a duration and a deadline in the form of _(t<sub>i</sub>, d<sub>i</sub>)_.
### Constraints


### Output Format
- A list of the requests with their start time and finish time in the form _(s<sub>i</sub>, f<sub>i</sub>)_

## Algorithm
### Overview
To minimize the lateness that the jobs run, it makes sense to schedule the ones with the earliest deadlines first.

### Pseudo Code

```Python
    def min_max_lateness(R: set of requests):
        1. Sort request by their deadline (earliest first)
        2. f = 0 # initialize 'Finish' to 0
        3. For every request i in R:
            a. start of i = f
            b. finish of i = start of i + time i requires to finish
            c. f = f + time i requires to finish
        4. Return [s(i), f(i)] for every i
```

### Analysis
This algorithm is much like interval scheduling and interval partitioning. The time complexity is largely influenced by that of the
sorting algorithm used. In this case we will use quick sort, which has an average time complexity of O(nlogn), and a worst case of O(n<sup>2</sup>).
This will give our scheduling an average time complexity of O(nlogn).


### Example

    Initial set of requests: (3, 10), (4, 5), (1, 6), (6, 2), (1, 11)

    Step 1 - Sort Requests by deadline
        Sorted Requests = (6, 2), (4, 5), (1, 6), (3, 10), (1, 11)

    Step 2 - Initialize variable to hold last finish time
        f = 0

    Step 2 - Loop through sorted requests and calculate start and finish time

    Iteration 1, request (6, 2):
        * start<sub>i</sub> = 0
        * finish<sub>i</sub> = start<sub>i</sub> + 6
        * f = f + 6
        * schedule for this request: [(0, 6)]

    Iteration 2, request (4, 5):
        * start<sub>i</sub> = 6
        * finish<sub>i</sub> = start<sub>i</sub> + 4
        * f = f + 4
        * schedule for this request: [(6, 10)]

    Iteration 3, request (1, 6):
        * start<sub>i</sub> = 10
        * finish<sub>i</sub> = start<sub>i</sub> + 1
        * f = f + 1
        * schedule for this request: [(10, 11)]

    Iteration 4, request (3, 10):
        * start<sub>i</sub> = 11
        * finish<sub>i</sub> = start<sub>i</sub> + 3
        * f = f + 3
        * schedule for this request: [(11, 14)]

    Iteration 5, request (1, 11):
        * start<sub>i</sub> = 14
        * finish<sub>i</sub> = start<sub>i</sub> + 1
        * f = f + 1
        * schedule for this request: [(14, 15)]

    Done. The completed schedule is:

| Request (t<sub>i</sub>, d<sub>i</sub>)  | (Start, Finish)  |
|----|--------------|
| (6, 2) | (0, 6) |
| (4, 5) | (6, 10) |
| (1, 6) | (10, 11) |
| (3, 10) | (11, 14) |
| (1, 11) | (14, 15) |


## Conclusion
The schedules are now sorted and each have a start and finish time that will minimize their lateness (finish time - deadline).

