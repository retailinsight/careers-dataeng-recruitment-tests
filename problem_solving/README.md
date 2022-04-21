# DATA ENGINEER TECHNICAL TEST

## THE SCENARIO

You will be given a problem that involves verifying data arriving in a stream. We’ll cover this as soon as the interview commences. During the interview, you can ask any questions you like provided they are specific i.e. you cannot ask, what is a good solution to this problem! You are able to use any resources you like such as Google, if you think it is necessary. Your focus should be exploring the problem and designing a solution. The interviewer does not need to see working code. The interviewer will ask you to explain your thinking as you go, but will not be contributing to your design.

## WE ARE LOOKING AT THE FOLLOWING CHARACTERISTICS

*   Comprehension – Your ability to comprehend the problem and broader aspects in a reasonable timeframe.
*   Reduction and refinement - How well you questions and probe the problem to refine understanding.
*   Design and reasoning - What are the reasons and assumptions behind the design, what objectives and constraints are you designing for.
*   Technical breadth - Your ability to see and design for broader aspects of the problem space, perhaps even outside of the problem space.
*   Technical depth - Your ability to think deeply about the problem and solution i.e. a technically comprehensive or sophisticated solution.
*   Communication - Your ability to communicate design thinking and technical thinking, clearly and logically to the interviewer throughout the exercise.

## THE TECHNICAL BRIEF
Given a system in which devices send a stream of events, design a system to detect if any devices have stopped sending data. Examples of such systems might be weather stations, car park sensors or machine sensors in large manufacturing facilities. 

## RULES
*   You cannot modify the delivery protocol or schema of the data, you must rely on what is being received. 
*   New devices might be added at any time and should not require manual setup. 
*   Each device is independent from each other. 
*   You can assume that all event clocks are accurate
*   You can assume each device sends its data in time sequence. 

**The event stream has the following schema:**

  |device_id|event_time|event_data|
  |---|---|---|
  |a00000001|2020-02-01T10:01:00+10:00|{state: occupied}|
  |a00000001|2020-02-01T10:04:07+10:00|{state: unoccupied}|
  |a00000001|2020-02-01T10:06:54+10:00|{state: occupied}|
  |a00000001|2020-02-01T10:08:00+10:00|{state: unoccupied}|

## SUGGESTED APPROACH

You can use whatever approach you like, this is provided as a suggested guide.  Read this fully and then restate your understanding of the problem to the interviewer.

1. How would you determine what ‘stopped sending data’ actually means? Is it a constant time threshold, or does it vary by event time or device?
1. Model the data. How would you go about finding patterns in the data?
1. There could be many models of this problem with varying complexity, explain one or more possible models?
1. How might you implement the chosen solution?
1. What technologies might you use?
1. What challenges can you foresee?
1. Pick one of these challenges and explain how might you overcome it?
1. How would you test or otherwise ensure the ongoing quality of your solution?
1. How would you deploy your solution?

