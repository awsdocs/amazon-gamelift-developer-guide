# How GameLift FleetIQ works<a name="queues-design-fleetiq"></a>

FleetIQ is the GameLift algorithm that manages how a game session queue selects hosting resources for game session placements\. A queue is configured with specific fleet destinations where game sessions can be placed\. FleetIQ relies on the following decisionmaking process when searching for the best possible placement for a new game session\. 

1. FleetIQ filters the queue's destinations to remove any of the following fleets:
   + If the game session placement request provides player latency data, FleetIQ evaluates the queue's player latency policies and removes all fleets in Regions where the latency that is reported by a player in the request exceeds a policy's maximum limit\. 
   + FleetIQ removes any Spot fleets that are not currently viable due to unacceptable interruption rates\.

1. FleetIQ prioritizes the remaining queue destinations based on the following:
   + If player latency data is provided, FleetIQ re\-orders the queue destinations by Region, those with lowest average player latency listed first\.
   + If player latency data is **not** provided, FleetIQ uses the original list of queue destinations\.

1. FleetIQ selects a destination from the prioritized list\. 
   + If the destination list was prioritized by Region, FleetIQ selects the fleet in the lowest\-latency Region with the lowest price\. If there are no viable Spot fleets, any fleet in that Region may be selected\.
   + If the destination list was **not** prioritized, FleetIQ selects the first viable fleet from the original list, even if there are lower priced Spot fleets on the list\.

1. FleetIQ evaluates whether the selected fleet has a server process available to host a new game session\. It is considered an "optimal" placement when the new game session is placed in a fleet with the lowest possible latency and/or the lowest price\. 

1. If the selected fleet has no available resources, FleetIQ moves to the next listed destination and repeats until it finds a fleet to host the new game session\.

    