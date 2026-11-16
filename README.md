## Project Description

The project spawns 10,000 `BP_TestActor` objects. Each actor gets a random `Score` value.

The GameManager gets the total score from all of the actors.

## Gameplay Loop

- 10,000 `BP_TestActor` objects are spawned.
- Each actor has a random `Score`.
- The GameManager gets the total score.
- The total score is printed during gameplay.

## Before Optimization

Before the optimization, the GameManager calculates the total score every frame.

The GameManager uses:

- `Event Tick`
- `Get All Actors Of Class`
- `For Each Loop`
- `Cast To BP_TestActor`
- `Score`
- `Current Score`

It finds all 10,000 TestActors, gets the score from each actor and adds them together.

### Blueprint Before
<img width="755" height="298" alt="Screenshot 2026-11-16 180639" src="https://github.com/user-attachments/assets/35fc62ca-12c0-4017-9f9a-5f6fa5e88012" />
<img width="459" height="196" alt="Screenshot 2026-11-16 180647" src="https://github.com/user-attachments/assets/e05f994d-a18b-484b-bbe1-f980ae377653" />
<img width="950" height="487" alt="Screenshot 2026-11-16 180627" src="https://github.com/user-attachments/assets/e75192d7-2601-4853-a5a5-18f35ed24ab8" />


### Profiler Before

<img width="957" height="565" alt="Screenshot 2026-11-16 175255" src="https://github.com/user-attachments/assets/c2976116-7e8a-430c-83c9-5f28107e620b" />


Profiler results:

- Selected Time: 4.98 seconds
- WorldTick Count: 1,938
- WorldTick Inclusive Time: 165.56 ms
- WorldTick Average: 0.684 ms
- WorldTick Maximum: 2.644 ms

## After Optimization

For the optimized version, I used the Singleton Pattern with a custom GameInstance called `BP_GI`.

Instead of the GameManager searching through all of the actors every frame, each `BP_TestActor` adds its score to the shared `Score` variable in `BP_GI`.

The GameManager then gets the stored score using:

- `Get Game Instance`
- `Cast To BP_GI`
- `Get Score`

This removes the need to find and loop through all 10,000 actors every frame.

### BP_TestActor Blueprint

<img width="895" height="279" alt="image" src="https://github.com/user-attachments/assets/3908c390-86a4-46f4-89e7-cd5d95dbb340" />


### GameManager Blueprint

<img width="933" height="437" alt="image" src="https://github.com/user-attachments/assets/275049f1-86c0-4d46-bd71-85310ecfcb55" />


### Profiler After

<img width="961" height="569" alt="Screenshot 2026-11-16 175301" src="https://github.com/user-attachments/assets/d71188d4-562d-446e-a0ed-6378f8207e66" />


Profiler results:

- Selected Time: 5.01 seconds
- WorldTick Count: 135
- WorldTick Inclusive Time: 4.08 ms
- WorldTick Average: 0.272 ms
- WorldTick Maximum: 0.423 ms

## Reflection

### What was impacting the performance?

The GameManager was using `Get All Actors Of Class` every frame and looping through 10,000 TestActors. It also had to cast to every actor and add their score values together every frame.

### Which pattern did you implement and what did it replace?

I used the Singleton Pattern with `BP_GI`.

It replaced the system where the GameManager repeatedly searched for all TestActors and calculated the score every frame.

The score is now stored in one shared variable inside the GameInstance.

### Did the pattern improve performance?

Yes.

The average WorldTick time went from 0.684 ms before the optimization to 0.272 ms after the optimization.

The maximum WorldTick time also went from 2.644 ms to 0.423 ms.

This shows that removing the repeated actor search and loop reduced the amount of work being done.

### What did the profiler show?

Unreal Insights showed more WorldTick processing before the optimization.

Before the change, WorldTick had 165.56 ms of inclusive CPU time during the selected period.

After using the GameInstance, it went down to 4.08 ms during a similar selected period.

The profiler showed that directly getting the stored score was more efficient than searching through all 10,000 actors every frame.


