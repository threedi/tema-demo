# Altenahr road tunnel shortcut

To the east of Altenahr, at the top right in below map, a road bridge provided a shortcut for the bend in the Ahr river:

![](blockage-at-bridge-left-and-shortcut-tunnel-right.png)

(Black: railway, blue: big bend in the Ahr river, red arrow top right: the road tunnel.) (Bottom left stuff: [see other case study](https://github.com/threedi/tema-demo/blob/main/example-case-study/case-study.md).)

## Situation

![Landesarchiv Baden-Württemberg, Staatsarchiv Freiburg W 134 Nr. 024164c / Fotograf: Willy Pragher, CC BY 4.0 &lt;https://creativecommons.org/licenses/by/4.0&gt;, via Wikimedia Commons](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Altenahr-_Tunnels_bei_Altenahr_%28Engelslay-I-Tunnel%2C_Engelslay-II-Tunnel_und_Stra%C3%9Fentunnel%2C_Westportale%29_-_LABW_-_Staatsarchiv_Freiburg_W_134_Nr._024164c.jpg/512px-Altenahr-_Tunnels_bei_Altenahr_%28Engelslay-I-Tunnel%2C_Engelslay-II-Tunnel_und_Stra%C3%9Fentunnel%2C_Westportale%29_-_LABW_-_Staatsarchiv_Freiburg_W_134_Nr._024164c.jpg?20250803103519)


The image above shows the situation, viewed towards the east, with the two railway tunnels (the left one is currently a cycling path) and, situated a bit lower to the right, the road tunnel.

The image below shows the situation after the flood:

![Ogmios, CC0, via Wikimedia Commons"](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/2021-08-21_Altenahr_04.jpg/1024px-2021-08-21_Altenahr_04.jpg?20210823094213)


## The flood in 2021

The river got so high that it could use the road tunnel to take a shortcut. Here are two youtube shorts that show it from the other side of the road tunnel:

- https://www.youtube.com/shorts/JlJWLDYZNa4
- https://www.youtube.com/shorts/X0mjNDGuWno (this one shows the shortcut ending up in the original stream).

Lastly, [this video](https://www.youtube.com/watch?v=NmS4iDpWlpU) shows construction works after the flood. 30 seconds into the video you get an impression of the regular streambed of the Ahr.

As an **happier interlude**, here's a [short video](https://youtu.be/KwMq6hQiEng) of my kids cycling through the cycling tunnel at this location (2019) :-)


## Technical relevance

At Nelen & Schuurmans, our flood simulation software works based on square grid cells. The basic grid cell can be further subdivided in smaller cells when needed. But, the more grid cells, the slower the simulation. Especially for emergencies, a good, quick simulation is of the essence.

If a too-big grid cell near the tunnel would cover both sides of the ridge at that point, the calculation software would treat the water on both sides of the ridge as connected, which is wrong. So: at this point, grid refinement would normally be needed.

One thing we're focusing on in this EU project is "clone cells". A cell that is cloned, with the two new cells each covering only half the cell, split along some line (like the ridge line). That would mean we could use bigger cell sizes, resulting in quicker calculations.

The ridge in this example is a nice location to test this out! With the added difficulty of modelling the road tunnel that *does* connect the two sides at a certain water level.
