# Time, Clocks and the ordering of Events in a Distributed System.

## Ordered thoughts:
* The abstract makes me think of my past thought about having the concept of time in a ds. Would it be advantageous having the concept of time in a ds? Who will manage it? How will it stay in sync etc.
* They define a system as a ds when the delay between the "split systems" is not _negligible_ anymore. Somewhat different to what I thought before - _identical_, _multiple_ nodes working together for the same goal.
* They claim that we cannot have a _temporal ordering_ of events in a ds - probably due to the fact that each node might percieve time in a different way.
* The paper claims to do a lot of things - find out a partial ordering of events, use a _distributed algorithm_ to convert the said partial ordering to a total ordering and then introduce a way to sync clocks in the nodes and also have an upper bound of the _drift_ between these clocks.
* _The sense of time in a system is defined by the events happenning inside it_ - paraphrasing but WOW.
* There's an error in pg 559 col 2 last line of first paragraph - `q1` instead of `p1`.
* They introduce clocks by telling that the "clock" is just a function that returns a number which determines the order of the event. This made me look into [the time package](https://github.com/golang/go/blob/master/src/time/time.go) in golang.
* I think the clock definition also implies that even if `C1(a) == C2(b)` it doesn't mean that `a` and `b` occurred at the same time.
* Figuring out how Go does it has been unsuccesful. I find a `startTimer` function which doesnt seem to have been implemented but used. My guess would be a link to the operating system and utilise its sense of time.
* Total ordering is done based on which process came first and internally which event was triggerred first.

## Overall thoughts:
nil
