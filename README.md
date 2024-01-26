# Speed up particle collision detection through hash tables #

## General problem ##
A simple particle simulation simulates a **set of particles that move freely in two-dimensional space until they collide with another particle** or with the environment. In the event of a collision
particles are deflected according to a simple model. The simulation takes place over discrete time steps. In each step, the particles are moved further according to their velocity, and in the case of collisions collisions, the new speed and direction are also calculated.

A **naive implementation** of collision detection **checks each pair of particles for a possible collision**. The cost of **collision detection is therefore O(n^2)** , where n is the number of particles. With a larger number of particles, the effort required to calculate each step in the simulation is quickly dominated by the collision detection.

![Screenshot of particle collision detection in action](https://github.com/oliolioli/SpeedUpCollisionDetection-through-HashTables/blob/main/bouncing.png)

## Making collision detection a lot faster with hash tables ##
The idea is now to use a data structure that manages the particles according to their spatial position. Because the particles are randomly distributed in space, the **position of a particle can be used to calculate a hash value**. Given the x-coordinate of the particle in the range 0 ≤ x < w, the **hash value is h(x) = floor(xm/w)**, where m is the size of the hash table. A hash value can also be calculated for the y-coordinate. The two hash values can now be used as indices in a two-dimensional hash table.

The **two-dimensional hash table corresponds to a two-dimensional grid**: each particle is assigned to a grid cell according to its position. This allows collision detection to be made more efficient by only analysing each particle for collisions in its own grid cell and the immediate neighbouring cells. The neighbouring cells must be tested because particles have a certain size and can overlap with the neighbouring cells. After each simulation step, the particles must be removed from the hash table according to their new position and re-entered in the correct cell.

## Howto run the code ##
Simply compile *.java and run BouncingBalls 
(≥ openjdk-17.0.2)

## Speed test* & conclusion ##

| Timer / simulation step [ms]     | # Particles | Size of hash table [x*y] |
| ------- | -------------- | ---------------------------------------------- |
| ~6      | 1000           | Initial implementation, without hash table     |
| 0.31	  | 1000	         | 100*100                                        |
| 1.28	  | 100	           | 100*100                                  (c)   |
| 3.24	  | 1000	         | 10*10                                    (b)   |
| 0.09	  | 100	           | 10*10                                    (a)   |

**Findings:**
Distributing the particles as evenly as possible across the hash table leads to good performance.
Approximately one particle per bucket is probably an ideal size (c). Hash tables that are too small lead to lists that have to be iterated.
We see a hash table that is too small in measurement / setting (b) and a hash table that is too large in measurement / setting (c)

*Made on a 12year old Lenovo x201 with 8GB RAM.
