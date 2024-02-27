<h1>ExpNo 1 :Developing AI Agent with PEAS Description</h1>
<h3>Name: AISHWARYA.S</h3>
<h3>Register Number:212222100003</h3>


<h3>AIM:</h3>
<br>
<p>To find the PEAS description for the given AI problem and develop an AI agent.</p>
<br>
<h3>Theory</h3>
<h3>Vaccum Cleaner agent:</h3>
<p>The VacuumCleanerAgent is a Python class that simulates the behavior of a basic vacuum cleaner in a two-location environment ("A" and "B"). The agent can perform four actions: move left, move right, suck dirt, and do nothing. Its state includes the current location and dirt status in each location. The agent's initial state is at location "A" with no dirt. Actions like moving and sucking dirt can change its state, and the print_status method displays the current location and dirt status. This agent provides a foundation for simple vacuum cleaner simulations and can be adapted for more complex scenarios.</p>
<hr>
<h3>PEAS DESCRIPTION:</h3>
<table>
  <tr>
    <td><strong>Agent Type</strong></td>
    <td><strong>Performance</strong></td>
     <td><strong>Environment</strong></td>
    <td><strong>Actuators</strong></td>
    <td><strong>Sensors</strong></td>
  </tr>
    <tr>
    <td><strong>Vaccum Cleaner agent</strong></td>
    <td><strong>Cleaning Dirt</strong></td>
     <td><strong>Rooms, floor</strong></td>
    <td><strong>Dirt,Cleaning</strong></td>
    <td><strong>Location,Sensing Dirt</strong></td>
  </tr>
</table>
<hr>
<H3>DESIGN STEPS</H3>
<h3>STEP 1:Identifying the input:</h3>
<p> Location.</p>
<h3>STEP 2:Identifying the output:</h3>
<p>move_left: Moves the agent to the left if it is currently at location "B.".
move_right: Moves the agent to the right if it is currently at location "A."
suck_dirt: Sucks dirt in the current location if there is dirt present.
After sucking dirt, the dirt status in that location is updated to indicate cleanliness.
do_nothing: Represents a passive action where the agent remains idle.</p>
<h3>STEP 3:Developing the PEAS description:</h3>
<p>PEAS description is developed by the performance, environment, actuators, and sensors in an agent.</p>
<h3>STEP 4:Implementing the AI agent:</h3>
<p>Clean the room and Search for dirt and Suck it.</p>
<h3>STEP 5:</h3>
<p>Measure the performance parameters: For each cleaning performance incremented, for each movement performance decremented</p>
<h3>CODE:</h3>

<code>

import random
import time
class Thing:
 """
 This represents any physical object that can appear in an Environment. 
 def is_alive(self):
 """Things that are 'alive' should return true."""
 return hasattr(self, "alive") and self.alive
 def show_state(self):
 """Display the agent's internal state. Subclasses should override."
 print("I don't know how to show_state.")

class Agent(Thing):
 
 """
 An Agent is a subclass of Thing """
 def __init__(self, program=None):
 self.alive = True
 self.performance = 0
 self.program = program
 def can_grab(self, thing):
 """Return True if this agent can grab this thing. Override for appr
 return False
In [8]:
def TableDrivenAgentProgram(table):
 """
 
 This agent selects an action based on the percept sequence. It is pract
 To customize it, provide as table a dictionary of all
 {percept_sequence:action} pairs. """
 percepts = []
 
 def program(percept):
 action = None
 percepts.append(percept)
 action = table.get(tuple(percepts))
 return action
 return program

loc_A, loc_B = (0,0), (1,0) # The two locations for the Vaccum cleaning 

def TableDrivenVaccumAgent():
 """
 Tabular approach towards Vaccum cleaning
 """
 
 table = {
 ((loc_A, "Clean"),): "Right",
 ((loc_A, "Dirty"),): "Suck",
 ((loc_B, "Clean"),): "Left",
 ((loc_B, "Dirty"),): "Suck",
 ((loc_A, "Dirty"), (loc_A, "Clean")): "Right",
 ((loc_A, "Clean"), (loc_B, "Dirty")): "Suck",
 ((loc_B, "Clean"), (loc_A, "Dirty")): "Suck",
20/02/2024, 22:25 VACCUM-Copy1
localhost:8888/nbconvert/html/Downloads/VACCUM-Copy1.ipynb?download=false 2/4
 ((loc_B, "Dirty"), (loc_B, "Clean")): "Left",
 ((loc_A, "Dirty"), (loc_A, "Clean"), (loc_B, "Dirty")): "Suck",
 ((loc_B, "Dirty"), (loc_B, "Clean"), (loc_A, "Dirty")): "Suck",
 }
 return Agent(TableDrivenAgentProgram(table))

class Environment:
 """Abstract class representing an Environment. 'Real' Environment classe
 percept: Define the percept that an agent sees. execute_action: Def
 Also update the agent.performance slot.
 The environment keeps a list of .things and .agents (which is a subset 
 Each thing has a .location slot, even though some environments may not 
 def __init__(self):
 self.things = []
 self.agents = []
 
 def percept(self, agent):
 """Return the percept that the agent sees at this point. (Implement 
 raise NotImplementedError
 def execute_action(self, agent, action):
 """Change the world to reflect this action. (Implement this.)"""
 raise NotImplementedError
 def default_location(self, thing):
 """Default location to place a new thing with unspecified location.
 return None
 def is_done(self):
 """By default, we're done when we can't find a live agent."""
 return not any(agent.is_alive() for agent in self.agents)
 def step(self):
 """Run the environment for one time step. If the
 actions and exogenous changes are independent, this method will do. 
 if not self.is_done():
 actions = []
 for agent in self.agents:
 if agent.alive:
 actions.append(agent.program(self.percept(agent)))
 else:
 actions.append("")
 for (agent, action) in zip(self.agents, actions):
 self.execute_action(agent, action)
 def run(self, steps=1000):
 """Run the Environment for given number of time steps."""
 for step in range(steps):
 if self.is_done():
 return
 self.step()
 def add_thing(self, thing, location=None):
 """Add a thing to the environment, setting its location. For conven
 if not isinstance(thing, Thing):
 thing = Agent(thing)
 if thing in self.things:
 print("Can't add the same thing twice")
 else:
 thing.location = (location if location is not None else self.de
 self.things.append(thing)
 if isinstance(thing, Agent):
20/02/2024, 22:25 VACCUM-Copy1
localhost:8888/nbconvert/html/Downloads/VACCUM-Copy1.ipynb?download=false 3/4
 thing.performance = 0
 self.agents.append(thing)
 def delete_thing(self, thing):
 """Remove a thing from the environment."""
 try:
 
 self.things.remove(thing)
 except ValueError as e:
 print(e)
 print(" in Environment delete_thing")
 print(" Thing to be removed: {} at {}".format(thing, thing.locat
 print(" from list: {}".format([(thing, thing.location) for thing
 if thing in self.agents:
 self.agents.remove(thing)

class TrivialVaccumEnvironment(Environment):
 """This environment has two locations, A and B. Each can be clean or di
 def __init__(self):
 super().__init__()
 #loc_A, loc_B = (0,0), (1,0) # The two locations for the Vaccum clea
 self.status = {loc_A: random.choice(["Clean", "Dirty"]), loc_B: rand
 def thing_classes(self):
 return [TableDrivenVaccumAgent]
 def percept(self, agent):
 """Returns the agent's location, and the location status (Dirty/Cle
 return agent.location, self.status[agent.location]
 def execute_action(self, agent, action):
 """Change agent's location and/or location's status; track performa
 if action == "Right":
 agent.location = loc_B
 agent.performance -= 1
 elif action == "Left":
 agent.location = loc_A
 agent.performance -= 1
 elif action == "Suck":
 
 if self.status[agent.location] == "Dirty":
 agent.performance += 10
 self.status[agent.location] = "Clean"
 def default_location(self, thing):
 
 return random.choice([loc_A, loc_B])

if __name__ == "__main__":
 
 agent = TableDrivenVaccumAgent()
 environment = TrivialVaccumEnvironment()
 #print(environment)
 environment.add_thing(agent)
 print(environment.status)
 environment.run(steps=10)
 print(environment.status)
 print(agent.performance)

</code>

<h3>OUTPUT:</h3>

![WhatsApp Image 2024-02-20 at 22 19 39_181f12bb](https://github.com/Nandhakumar1313/19AI405ExpNo1/assets/120230694/0e780de5-71dd-4d61-a02a-a709d59c2851)

