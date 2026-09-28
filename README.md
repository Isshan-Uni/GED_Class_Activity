# Obstacle Course - Factory Pattern

**Name:** Isshan Marwah  
**Student Number:** 100989890

## Project Description

This is a simple 3D obstacle-course game made in Unreal Engine 5.8. The player has to run and jump across platforms while avoiding different hazards. Some hazards damage the player, some push the player, and another can defeat the player in one hit.

The player can also collect Health and Speed powerups. The goal is to reach the end of the level without losing all health or falling into the lose area.

For this activity, I added a Factory Pattern to control how the powerups are spawned.

<img width="851" height="335" alt="image" src="https://github.com/user-attachments/assets/bcdc5a56-c01e-4df9-8792-d6ec2de62085" />

### Note

For this build, I have not added a full UI/HUD yet. Instead, gameplay actions such as taking damage, healing, collecting a powerup, and receiving a speed boost are confirmed using Print String messages on screen. The win and lose conditions also display a message first, then close the game after a short delay.

I did not have enough time to create the full UI for this activity, but I plan to add and polish it before the next activity.


## Factory Pattern

I created a parent Blueprint called BP_PowerupFactory. It contains a PowerupClass variable and a SpawnPowerup function.

I then created two child factories:

- BP_HealthFactory - spawns the Health Powerup
- BP_SpeedFactory - spawns the Speed Powerup

Both children use the same spawning logic from BP_PowerupFactory, but their PowerupClass variable is set to a different powerup.

The BP_FirstPersonGameMode stores the available factories and controls when they are used. At the start of the game, two random factories are selected to spawn powerups.

When the player's health goes below 50, the player calls RequestHeal in the GameMode. The GameMode searches the remaining factories, finds the closest available BP_HealthFactory, and tells it to spawn a Health Powerup.

Once a factory has been used, it is removed from the available factory array and destroyed so the same spawn location cannot be used again.

### BP_PowerupFactory - SpawnPowerup Function
<img width="971" height="496" alt="image" src="https://github.com/user-attachments/assets/dad11bcf-40a2-4f54-8161-a16af896e127" />

### Health and Speed Factory Child Values
<img width="860" height="144" alt="image" src="https://github.com/user-attachments/assets/57bc33f8-69e9-4fa0-8772-0d130c1c9c12" />
<img width="758" height="148" alt="image" src="https://github.com/user-attachments/assets/2be144ee-1822-4e6b-8919-27bf0c120179" />

### GameMode Functions
<img width="1043" height="360" alt="image" src="https://github.com/user-attachments/assets/cef2fda6-1fb4-440c-8b11-2bb8905904c6" />
<img width="1274" height="437" alt="image" src="https://github.com/user-attachments/assets/477716c1-2fbb-4948-b1f7-0795004c1b49" />


## Factory Pattern Diagram

<img width="958" height="587" alt="image" src="https://github.com/user-attachments/assets/222a8c7e-80de-48a0-a5b6-458eaf77617e" />
The roles in the Factory Pattern are:

- BP_PowerupFactory - Creator / Base Factory
- BP_HealthFactory and BP_SpeedFactory - Concrete Factories
- BP_PowerUp - Base Product
- BP_PowerUp_Child_HealUp and BP_PowerUp_Child_SpeedUp - Concrete Products
- BP_FirstPersonGameMode - Client / Manager that decides when a Factory should be used

The Factory children inherit the SpawnPowerup function from BP_PowerupFactory, while each child changes the PowerupClass value to determine which powerup gets created.

## Reflection

### What element of your game adopts the chosen pattern?

The powerup spawning system uses the Factory Pattern. BP_PowerupFactory contains the common spawning functionality, while its child factories decide which type of powerup is spawned.

### Why is this pattern a good choice for the associated functionality?

The Factory Pattern works well because I have multiple powerups that use the same spawning process. Instead of writing separate spawning logic for each powerup, the parent Factory handles the spawning and the child Factories only decide what gets spawned.

It also makes the system easier to expand because I can add another powerup and Factory child later without changing the main spawning system.

## External Assets

No external assets were used.

## Release

A packaged Windows build of the game is available in the **Releases** section of this repository.
