# Stateless vs Stateful 
- stateful means having a state (obviously) while stateless is the opposite 
- it is common to have a stateful parent that maintains its own state
  - the parent then passes their own state down to stateless children as props 
- a component should never change its own props 
  - however, state can store information that a component itself can update 
  -
