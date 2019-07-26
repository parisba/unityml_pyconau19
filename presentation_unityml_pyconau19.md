footer: @parisba / #pyconau
theme: Business Class


# Building, designing, teaching and training simulation environments for Machine Learning

## Paris Buttfield-Addison

---

# Paris Buttfield-Addison

- game developer
- PhD in Computer Science
- machine learning enthusiast

![right 100%](presentation_images/parisba.png)

---

<!-- https://2019.pycon-au.org/talks/building-designing-teaching-and-training-simulation-environments-for-machine-learning -->

<!-- Imagine you’re building a fancy robot-driven warehouse. Your pick, place, and packing robots need to get around quickly, find the right item and put it to the right place without colliding with each other, shelves, or people. But you don’t have any robots yet, and you need to start. Try simulations!

Imagine you’re building a high-volume, expensive, robot-driven warehouse. Your pick, place, and packing robots need to get to the right place quickly, find the right item and sort it to the right place without colliding with each other, the shelves, or people. But you don’t have any robots yet, and you need to start writing the logic. Enter: game engines.

A game engine is controlled, self-contained spatial, physical environment that (can) closely replicate (enough of) the real world (to be useful).

This session will explore how you can use game engines to explore and solve problems——such as the aforementioned robot-driven warehouse——in a simulated environment, without building costly and complicated real-world rigs.

Learn how:
 * straightforward a game engine can be 
 * how to define and lay out your environment for ML problem solving 
 * how to choose and apply different ML techniques, algorithms, and learning approaches, such as (deep) reinforcement learning, imitation learning, and curriculum learning 
* how generic but powerful Python-powered algorithms, such as PPO (Proximal Policy Optimization) make this new-wave of ML possible * how to tune simulations and pick the right actions, observations, and rewards to optimise your agents 
* how to design for emergent problem solving by your agents, where you’re not quite sure what the optimal solution looks like

Using the popular, powerful (Mono/C#-based) Unity game engine, and its integration with Python, primarily for TensorFlow, as a case study, this session will explore simulation-driven machine learning.

Game engines are a great place to explore ML and AI. They’re wonderful constrained problem spaces, tiny little ecosystems for you to explore a problem in. Here you can learn how to use them even though you’re not a game developer. It’s a great place to dive in to all the wonderful Python-powered ML- and AI- tools if you’re new to that space. It’s a little bit creative, too. -->

---

![fit](presentation_images/mlagents.png)

---

[.build-lists: true]

# What you need

1. Create a Python 3.6 environment and activate:
   - `conda create -n UnityMLEnv python=3.6`
   - `conda activate UnityMLEnv`
2. Install TensorFlow 1.7.1:
   - `pip install tensorflow==1.7.1`
3. Install ML-Agents:
   - `pip install mlagents`

^ After you're got Unity, what do you need?

---

# You need a problem

---

# Brain

---

# Brain
# Academy

---

# Brain
# Academy
# Agent

---

# Robot Warehouse

![alt](presentation_images/beepo.jpg)

---

![alt](presentation_images/beepo.jpg)

---

[.autoscale: true]

# The Process

![right](presentation_images/beepo.jpg)

1. Define a Problem
2. Build an Environment
3. Create an Agent
4. Pick an Approach
5. Implement Actions
6. Implement Observations
7. Implement Rewards
8. Train, Test, Repeat
 
---

TODO: the steps that go here 

---

# Implementing Actions

```csharp
public void MoveAgent(float[] act)
{
  Vector3 dirToGo = Vector3.zero;
  Vector3 rotateDir = Vector3.zero;

  int action = Mathf.FloorToInt(act[0]);

  switch(action) {
      case 1:
          dirToGo = transform.forward * 1f;
          break;
      case 2:
          dirToGo = transform.forward * -1f;
          break; 
      case 3:
          rotateDir = transform.up * 1f;
          break;
      case 4:
          rotateDir = transform.up * -1f;
          break;
      case 5:
          dirToGo = transform.right * -0.75f;
          break;
      case 6:
          dirToGo = transform.right * 0.75f;
          break;           
  }
  transform.Rotate(rotateDir, Time.fixedDeltaTime * 200f);
  agentRB.AddForce(dirToGo * academy.agentRunSpeed, ForceMode.VelocityChange);
}
```

^ This is a bit of a long one, so we'll go through it in pieces instead.

---

# Implementing Actions

[.code-highlight: all]
[.code-highlight: 1-2, 9]
[.code-highlight: 3]
[.code-highlight: 4]
[.code-highlight: 6]
```csharp
public void MoveAgent(float[] act)
{
	Vector3 dirToGo = Vector3.zero;
	Vector3 rotateDir = Vector3.zero;

	int action = Mathf.FloorToInt(act[0]);

	// ...more code...
}
```

^ We implement our actions in a function that we create, called MoveAgent. First, we need some temporary variables. Nothing particularly interesting.

---

# Implementing Actions

[.code-highlight: all]
[.code-highlight: 1-2, 15]
[.code-highlight: 3-4]
[.code-highlight: 5-6]
[.code-highlight: 7-8]
[.code-highlight: 9-10]
[.code-highlight: 11-12]
[.code-highlight: 13-14]



```csharp
switch(action) 
{
   case 1:
       dirToGo = transform.forward * 1f; break;
   case 2:
       dirToGo = transform.forward * -1f; break; 
   case 3:
       rotateDir = transform.up * 1f; break;
   case 4:
       rotateDir = transform.up * -1f; break;
   case 5:
       dirToGo = transform.right * -0.75f; break;
   case 6:
       dirToGo = transform.right * 0.75f; break;           
}
// ...more code...
```

---

# Implementing Actions

[.code-highlight: all]
[.code-highlight: 1]
[.code-highlight: 2]
[.code-highlight: 3]
[.code-highlight: all]


```csharp
transform.Rotate(rotateDir, Time.fixedDeltaTime * 200f);
agentRB.AddForce(dirToGo * academy.agentRunSpeed, ForceMode.VelocityChange);
```

---

# Implementing Actions

[.code-highlight: all]
[.code-highlight: all]
[.code-highlight: 3-6]
[.code-highlight: 8-21]
[.code-highlight: 23-24]
[.code-highlight: all]

```csharp
public void MoveAgent(float[] act)
{
  Vector3 dirToGo = Vector3.zero;
  Vector3 rotateDir = Vector3.zero;

  int action = Mathf.FloorToInt(act[0]);

  switch(action) {
      case 1:
          dirToGo = transform.forward * 1f; break;
      case 2:
          dirToGo = transform.forward * -1f; break; 
      case 3:
          rotateDir = transform.up * 1f; break;
      case 4:
          rotateDir = transform.up * -1f; break;
      case 5:
          dirToGo = transform.right * -0.75f; break;
      case 6:
          dirToGo = transform.right * 0.75f; break;  
  }

  transform.Rotate(rotateDir, Time.fixedDeltaTime * 200f);
  agentRB.AddForce(dirToGo * academy.agentRunSpeed, ForceMode.VelocityChange);
}
```

---

# Implementing Observations

---

# Implementing Observations

[.code-highlight: all]
[.code-highlight: 1-2, 8]
[.code-highlight: 3]
[.code-highlight: 4]
[.code-highlight: 5]
[.code-highlight: 6-7]

```csharp
public override void CollectObservations()
{
   var distance = 12f;
   float[] angles = { 0f, 45f, 90f, 135f, 180f, 110f, 70f };
   var objects = new[] { "crate", "goal", "wall" };
   AddVectorObs(rayPer.Perceive(distance, angles, objects, 0f, 0f));
   AddVectorObs(rayPer.Perceive(distance, angles, objects, 1.5f, 0f));
}
```
---

# Implementing Rewards

[.code-highlight: all]
[.code-highlight: 1-2, 5]
[.code-highlight: 3]
[.code-highlight: 4]
[.code-highlight: 3-4]
[.code-highlight: all]

```csharp
public override void AgentAction(float[] vectorAction, string textAction)
{
  MoveAgent(vectorAction);
  AddReward(-1f/ agentParameters.maxStep);
}
```

---

# Implementing Rewards

[.code-highlight: all]
[.code-highlight: 1-2, 4]
[.code-highlight: 3]
[.code-highlight: all]

```csharp
public void IHitWrongGoal(GameObject target, GameObject goal)
{
  AddReward(-5f);
}
```

---


# Implementing Rewards


[.code-highlight: all]
[.code-highlight: 1-2, 16]
[.code-highlight: 3-4]
[.code-highlight: all]
[.code-highlight: 6-15]
[.code-highlight: all]

```csharp
public void IScoredAGoal(GameObject target, GameObject goal)
{
  AddReward(5f);
  Debug.Log("Agent delivered crate!");

  var allGoalsComplete = true;
  foreach(var block in blocks) {
      if(block.IsActive == true) {
          allGoalsComplete = false;
      }
  }

  if(allGoalsComplete){
      Done();
  }
}
```

---