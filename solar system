from simulation import *


scale = input("chose the scale:"
              "\n1 - Sun - Mars"
              "\n2 - Sun - Neptune"
              "\n>")
if scale == "1":
    """ Sun - Mars """
    GRAPH_LIM = 227.9*(10**9)
    INTERVAL = 60*60*24
else:
    """ Sun - Neptune """
    GRAPH_LIM = 4.495*(10**12)
    INTERVAL = 60*60*24*7


# solar system !!!!!
# les coef sur les rayon son là juste pour qu'on voit les planets

Soleil = body_obj(1.989 * (10**30), [0, 0], [0, 0], color=(255,255,0), rayon=696340000 * 60)
Mercure =   body_obj(masse=3.285 * 10**23,   position=[57.91*(10**9), 0],  vitesse=[0, 175936/3.6], color=(174, 173, 149), rayon=4879000 * 1000)
Venus =     body_obj(masse=4.867 * 10**24, position=[108.2*(10**9), 0],  vitesse=[0, 126062/3.6], color=(139, 139, 132), rayon=12104000 * 1000)
Terre =     body_obj(masse=5.972 * 10**24, position=[149.6*(10**9), 0],  vitesse=[0, 108000/3.6], color=(39, 142, 245),  rayon=12742000 * 1000)
Mars =      body_obj(masse=6.39 * 10**23,    position=[227.9*(10**9), 0],  vitesse=[0, 87226/3.6],  color=(213, 210, 27),  rayon=6779000 * 1000)
Jupiter =   body_obj(masse=1.898 * 10**27,   position=[778.5*(10**9), 0],  vitesse=[0, 47196/3.6],  color=(221, 175, 32),  rayon=139820000 * 1000)
Saturne =   body_obj(masse=5.683 * 10**26,   position=[1.434*(10**12), 0], vitesse=[0, 34962/3.6],  color=(187, 218, 113), rayon=116460000 * 1000)
Uranus =    body_obj(masse=8.681 * 10**25,   position=[2.871*(10**12), 0], vitesse=[0, 24459/3.6],  color=(127, 226, 242), rayon=50724000 * 1000)
Neptune =   body_obj(masse=1.024 * 10**26,   position=[4.495*(10**12), 0], vitesse=[0, 19566/3.6],  color=(20, 58, 209),   rayon=49244000 * 1000)

planets = [Soleil, Mercure, Venus, Terre, Mars, Jupiter, Saturne, Uranus, Neptune]

sim = body_simulation(bodies=planets, time_interval=INTERVAL, DistanceToCenter=GRAPH_LIM, erase=True)
sim.start_animation()
