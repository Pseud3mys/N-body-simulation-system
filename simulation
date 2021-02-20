import numpy as np
import pygame

G = 6.67408 * 10 ** (-11)


def display_time(seconds, granularity=2):
    result = []
    _intervals = (
    ('years', 60 * 60 * 24 * 365), ('months', 60 * 60 * 24 * 30), ('weeks', 60 * 60 * 24 * 7), ('days', 60 * 60 * 24), ('hours', 60 * 60),
    ('minutes', 60), ('seconds', 1), ('microseconds', 0.001),)
    for name, count in _intervals:
        value = seconds // count
        if value == 0:
            continue
        seconds -= value * count
        if value == 1:
            name = name.rstrip('s')
        result.append("{} {}".format(int(value), name))
    return ', '.join(result[:granularity])


def _gravitation(bodyA, bodyB):
    """
    :param bodyA: class body
    :param bodyB: class body
    :return: f en N (force de gravitation)
    """
    x = pow(bodyA.position[0] - bodyB.position[0], 2) + pow(bodyA.position[1] - bodyB.position[1], 2)
    d = np.sqrt(float(x))
    # petit problemee
    if d == 0:
        d = 1
    F = G * (bodyA.mass * bodyB.mass) / pow(d, 2)
    return F


def _grav_to_speed(F, mass, time_interval):
    """
    :param F: en N
    :param mass: en kg
    :param time_interval: en second
    :return: en m.s
    """
    return F * time_interval / mass


class body_obj:
    def __init__(self, masse, position, vitesse, rayon, color):
        """
        :param masse: en kg
        :param position: array 2 elements; coordonnée X et Y
        :param vitesse: array 2 elements; vecteur vitesse
        :param diametre: en m
        :param color: color arg for matplotlib
        """
        self.mass = masse
        self.position = position  # array 2
        self.speed = vitesse  # array 2

        # save all the position of the body during the simulation
        self.X = []
        self.Y = []
        self.inScreenPos = []
        # array of grav Force of each other planet
        self.attractions = []
        # a unique index
        self.index = None
        # other param
        self.radius = rayon
        self.color = color

    def update(self, time_interval):
        # on somme les speeds de chaque attractions:
        for grav in self.attractions:
            self.speed[0] += _grav_to_speed(grav[0], self.mass, time_interval)
            self.speed[1] += _grav_to_speed(grav[1], self.mass, time_interval)
        #
        self.position[0] = self.position[0] + self.speed[0] * time_interval
        self.position[1] = self.position[1] + self.speed[1] * time_interval
        #
        self.X.append(self.position[0])
        self.Y.append(self.position[1])
        # reset
        self.attractions = []

    def draw(self, screen, screen_pxl, real_dist_max, doDrawLine):
        X = int(screen_pxl/2 * self.position[0] / real_dist_max + screen_pxl/2)
        Y = int(screen_pxl/2 * self.position[1] / real_dist_max + screen_pxl/2)
        pxlradius = int(screen_pxl * self.radius / real_dist_max)
        if doDrawLine:
            self.inScreenPos.append((X, Y))
        if pxlradius<1:
            pxlradius = 1
        pygame.draw.circle(screen, self.color, (X, Y), pxlradius)

    def DrawLines(self, screen):
        for Pos in self.inScreenPos:
            screen.fill(self.color, (Pos, (3, 3)))


class body_simulation:
    def __init__(self, bodies, time_interval, DistanceToCenter, line=True, erase=True):
        """
        :param bodies: an array of all bodies in the system
        :param time_interval: en s (resolution des calculs)
        :param graph_limits: X and Y limit for the plot
        :param line: bool; draw or not draw line
        :param vector: bool; draw or not draw vector
        """
        self.simulation_time = 0
        # list of the bodies
        self.bodies = bodies
        self.lines = line
        self.erase = erase
        # index them in order
        self.all_index = []
        index = 0
        for body in self.bodies:
            self.all_index.append(index)
            body.index = index
            index += 1
        # "-1" car il n'y aura jamais d'interaction entre le dernier de la list et qlq chose (vu que c'est le dernier)
        self.interactions = [[0 for _ in range(len(self.bodies))] for _ in range(len(self.bodies) - 1)]
        # time interval in seconds
        self.time_interval = time_interval

        self.graph_limit = DistanceToCenter
        #
        print("--- simulation of n bodies problem ---")
        print("- n = %d" % len(self.bodies))
        print("- time interval: %s" % display_time(self.time_interval))

    def update_bodies(self):
        """
        calcule toutes les interaction entre tout les astres,
        en faisant attention de faire qu'une seul fois le calcul.
        """
        # l'index de travail sera tjr plus petit que l'autre
        for start in range(len(self.all_index) - 1):
            index_left = self.all_index[start+1:]
            #
            work_index = self.all_index[start]
            work_body = self.bodies[work_index]
            for second_index in index_left:
                second_body = self.bodies[second_index]
                # force d'interaction de B sur A = interaction[A][B]
                F = _gravitation(work_body, second_body)

                # on a la vitesse de l'astre mais il manque encore la direction du vecteur
                coordX = self.bodies[second_index].position[0] - work_body.position[0]
                coordY = self.bodies[second_index].position[1] - work_body.position[1]

                # l'angle en radian
                angle = np.arctan(coordY / coordX)
                if coordX < 0:
                    angle += np.pi
                elif coordY < 0:
                    angle += np.pi * 2
                #
                gravX = F * np.cos(angle)
                gravY = F * np.sin(angle)
                # return the speed for the bodyA, and for the body B
                work_body.attractions.append([gravX, gravY])
                second_body.attractions.append([-gravX, -gravY])

            # un fois que c'est fait, o peut deja update le body sur lequel on a travaillé:
            work_body.update(self.time_interval)

        # update the last body
        self.bodies[len(self.all_index)-1].update(self.time_interval)

        #
        self.simulation_time += self.time_interval


    def start_animation(self, FPS=60):
        pygame.init()
        pxl = 680
        screen = pygame.display.set_mode((pxl, pxl))
        pygame.display.set_caption("N bodies simulation")
        clock = pygame.time.Clock()

        # black background
        screen.fill((0, 0, 0))

        while True:
            self.update_bodies()

            # Lock the framerate at 50 FPS
            clock.tick(FPS)

            # Handle events
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    print("\nSimulation ended after %d iterations. It's %s in real time" % (
                        self.simulation_time / self.time_interval, display_time(self.simulation_time)))
                    return
            if self.erase:
                screen.fill((0, 0, 0))

            if self.lines:
                for body in self.bodies:
                    body.DrawLines(screen)

            for body in self.bodies:
                body.draw(screen, pxl, self.graph_limit, self.lines)

            # mettre à jour la fenêtre
            pygame.display.update()
