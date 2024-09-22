# 🎮 Steering Behaviors in AI for Games

## 📝 Overview

This README introduces **Steering Behaviors** as an essential part of AI in games, focusing on how to build and understand intelligent, lifelike movement for characters. These behaviors combine basic rules to form more complex actions, allowing characters to react naturally to their surroundings.

We'll break down **basic steering behaviors**, explore how to **combine them into more complex systems**, and include an additional focus on the **Hide** behavior, which is useful in stealth mechanics.

## 🧠 Concept Map of Steering Behaviors


```mermaidmindmap
  root(("Steering Behaviors"))
    Basic_Behaviors
    ::icon(fa fa-cogs):::basic_behaviors
      Seek
      ::icon(fa fa-arrow-circle-up):::behavior
        description["Moves the agent toward a target"]
        relationships["Used in Pursue and Hide behaviors"]
      Flee
      ::icon(fa fa-arrow-circle-down):::behavior
        description["Moves the agent away from a target"]
        relationships["Used in Evade and Hide behaviors"]
      Wander
      ::icon(fa fa-random):::behavior
        description["Allows random movement for more lifelike behavior"]
      Pursue
      ::icon(fa fa-bullseye):::behavior
        description["Predicts the target's future position and moves toward it"]
        relationships["Built on Seek"]
      Evade
      ::icon(fa fa-shield-alt):::behavior
        description["Predicts the future position of a threat and moves away"]
        relationships["Built on Flee"]
      Hide
      ::icon(fa fa-eye-slash):::behavior
        description["Finds cover to avoid detection"]
        relationships["Uses Seek to move to hiding spots, combined with Flee or Evade"]

    Combining_Steering_Behaviors
    ::icon(fa fa-layer-group):::combining_behaviors
      Blending
      ::icon(fa fa-mix):::combine
        description["Combines multiple behaviors for smooth movement"]
        example["Example: Blending Seek and Flee"]
      Arbitration
      ::icon(fa fa-balance-scale-left):::combine
        description["Chooses the most appropriate behavior based on conditions"]
        example["Example: Switching between Pursue and Evade based on proximity"]
      Prioritization
      ::icon(fa fa-tasks):::combine
        description["Assigns importance to behaviors and executes the most urgent"]
        example["Example: Prioritizing Evade over Wander in case of danger"]

    Complex_Behaviors
    ::icon(fa fa-brain):::complex_behaviors
      Evade_and_Hide
      ::icon(fa fa-user-shield):::complex
        description["Combines Evade to avoid danger and Hide to seek cover"]
      Pursue_and_Wander
      ::icon(fa fa-question):::complex
        description["Mixes Pursue and Wander for unpredictable chasing behavior"]
      Blended_Caution
      ::icon(fa fa-exclamation-triangle):::complex
        description["Uses a blend of Seek and Flee for cautious movement"]

  %% Style Definitions
  classDef basic_behaviors fill:#f9f,stroke:#333,stroke-width:2px;
  classDef combining_behaviors fill:#ffec99,stroke:#333,stroke-width:2px;
  classDef complex_behaviors fill:#d1e7dd,stroke:#333,stroke-width:2px;
  classDef behavior fill:#c1e1ec,stroke:#036,stroke-width:2px;
  classDef combine fill:#f7c6c7,stroke:#630,stroke-width:2px;
  classDef complex fill:#b5ead7,stroke:#080,stroke-width:2px;

```

## ⚙️ Basic Steering Behaviors

### 1. 🚀 Seek
- **Objective**: Move toward a target.
- **How It Works**: The agent calculates the direction to the target and moves straight toward it.
- **Use Case**: Useful for direct movement such as chasing an enemy.

```csharp
void Seek(Vector3 location) {
    agent.SetDestination(location);
}
```

### 2. 🏃‍♂️ Flee

- Objective: Move away from a target.
- How It Works: The agent calculates the direction opposite to the target and moves in that direction.
- Use Case: Ideal for situations where the character must run away from danger.

```
void Flee(Vector3 location) {
    Vector3 fleeVector = location - this.transform.position;
    agent.SetDestination(this.transform.position - fleeVector);
}
```



### 🔀 Wander
- Objective: Move randomly within a space.
- How It Works: The agent picks random directions to move within a radius, giving it a more natural, lifelike movement.
- Use Case: Adds randomness to character movement, useful for NPCs.

```
void Wander() {
    Seek(targetWorld);
}
```


### 4. 🎯 Pursue
- Objective: Move toward where the target is going to be.
- How It Works: By predicting the target's future position, the agent intercepts the target.
- Use Case: Effective for chasing fast-moving targets.

```
void Pursue() {
    Seek(predictedPosition);
}
```

### 5. ⚡ Evade
- Objective: Move away from where the target is going to be.
- How It Works: Predicts the future location of the threat and avoids it.
- Use Case: Used for defensive maneuvers to avoid capture.

```
void Evade() {
    Flee(predictedPosition);
}
```

### 🛡️ Hide
- Objective: Find cover to avoid detection.
- How It Works: The agent looks for nearby objects to use as cover, positioning itself out of the line of sight of the target.
- Use Case: Crucial for stealth gameplay, where the AI must avoid detection.


```
void Hide() {
    // Calculate the best hiding spot behind a nearby obstacle.
    Vector3 hideSpot = CalculateBestHidingSpot(target, obstacle);
    Seek(hideSpot);
}
```


## 🔄 Combining Steering Behaviors

While individual behaviors are powerful, real-life movement often requires the combination of multiple behaviors to achieve more complex and intelligent actions.

### ⚖️ Blending
- Objective: Smoothly combine two or more behaviors.
- How It Works: By mixing outputs of different steering behaviors, you create a smoother, natural movement. For instance, mixing Seek and Flee based on the distance to a threat.


```
void BlendedSteering() {
    Vector3 blendedDirection = (Seek(target) * 0.7f) + (Flee(threat) * 0.3f);
    agent.SetDestination(blendedDirection);
}
```
### 🔄 Arbitration

- Objective: Choose the best behavior based on the situation.
- How It Works: You assign priority to different behaviors and decide which one takes control depending on game conditions. For example, Flee when close to danger, but Wander when safe.

### ⏫ Prioritization

- Objective: Assign priorities to behaviors to determine which behavior to use.
- How It Works: Certain behaviors take precedence based on game logic. Flee might have higher priority than Seek when danger is near.