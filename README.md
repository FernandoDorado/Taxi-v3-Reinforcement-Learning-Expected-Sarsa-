# Taxi-v3-Reinforcement-Learning-Expected-Sarsa

There are four specially-designated locations in
this world, marked as R(ed), B(lue), G(reen), and Y(ellow). The taxi problem is episodic. In
each episode, the taxi starts in a randomly-chosen square. There is a passenger at one of the
four locations (chosen randomly), and that passenger wishes to be transported to one of the four
locations (also chosen randomly). The taxi must go to the passenger’s location (the “source”), pick
up the passenger, go to the destination location (the “destination”), and put down the passenger
there. (To keep things uniform, the taxi must pick up and drop off the passenger even if he/she
is already located at the destination!) The episode ends when the passenger is deposited at the
destination location.
There are six primitive actions in this domain: (a) four navigation actions that move the taxi
one square North, South, East, or West, (b) a Pickup action, and (c) a Putdown action. Each action
is deterministic. There is a reward of −1 for each action and an additional reward of +20 for
successfully delivering the passenger. There is a reward of −10 if the taxi attempts to execute the
Putdown or Pickup actions illegally. If a navigation action would cause the taxi to hit a wall, the
action is a no-op, and there is only the usual reward of −1.
We seek a policy that maximizes the total reward per episode. There are 500 possible states:
25 squares, 5 locations for the passenger (counting the four starting locations and the taxi), and 4
destinations.
This task has a simple hierarchical structure in which there are two main sub-tasks: Get
the passenger and Deliver the passenger. Each of these subtasks in turn involves the subtask
of navigating to one of the four locations and then performing a Pickup or Putdown action.
This task illustrates the need to support temporal abstraction, state abstraction, and subtask
sharing. The temporal abstraction is obvious—for example, the process of navigating to the passenger’s location and picking up the passenger is a temporally extended action that can take different
numbers of steps to complete depending on the distance to the target. The top level policy (get
passenger; deliver passenger) can be expressed very simply if these temporal abstractions can be
9
employed.

The need for state abstraction is perhaps less obvious. Consider the subtask of getting the
passenger. While this subtask is being solved, the destination of the passenger is completely
irrelevant—it cannot affect any of the nagivation or pickup decisions. Perhaps more importantly,
when navigating to a target location (either the source or destination location of the passenger),
only the identity of the target location is important. The fact that in some cases the taxi is carrying
the passenger and in other cases it is not is irrelevant.
Finally, support for subtask sharing is critical. If the system could learn how to solve the
navigation subtask once, then the solution could be shared by both of the “Get the passenger”
and “Deliver the passenger” subtasks
