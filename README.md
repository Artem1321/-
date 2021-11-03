# -import vpython as vp
import math
import time
from vpython import sphere
import random

vp.scene.title = "проект"
vp.scene.height = 900
vp.scene.width = 1100

T = 0.00004
# m = float(input('масса: '))
# x = float(input('координата x(м): '))
# y = float(input('координата y(м): '))
# v = float(input('скорость(м/с): '))
# a=float(input('угол к горизонтали (0-360!), (если к солнцу, то -1): '))
Tru = 1
a = -1
m = 5.976 * (10 ** 24)
v = 65
x = -1 * (10 ** 8)
y = 0.5 * (10 ** 8)
q = 0
if x < 0:
    X = -1
    Y = -1
else:
    X = 1
    Y = 1
if a == -1:
    a = math.atan(y / x)
else:
    a = a * math.pi / 180
k1 = vp.sphere(pos=vp.vector(x, y, 0), radius=5000000, color=vp.vector(0.58, 0.153, 0.68),
               mass=m, momentum=vp.vector(-1 * m * v / T * math.cos(a) * X, -1 * m * v / T * math.sin(a) * Y, 0),
               make_trail=Tru)

star = vp.sphere(pos=vp.vector(0, 0, 0), radius=6963400, color=vp.color.yellow,
                 mass=1989000 * (10 ** 24), momentum=vp.vector(0, 0, 0), make_trail=Tru)

p1 = vp.sphere(pos=vp.vector(46 * (10 ** 6), 0, 0), radius=2439700, color=vp.color.green,
               mass=0.32868 * (10 ** 24), momentum=vp.vector(0, 47.87 * 0.32868 * (10 ** 24) / T, 0),
               make_trail=Tru)

p2 = vp.sphere(pos=vp.vector(107.5 * (10 ** 6), 0, 0), radius=5000000, color=vp.vector(0.0, 0.82, 0.33),
               mass=4.81068 * (10 ** 24), momentum=vp.vector(0, 35.02 * 4.81068 * (10 ** 24) / T, 0),
               make_trail=Tru)

p3 = vp.sphere(pos=vp.vector(147.1 * (10 ** 6), 0, 0), radius=5000000, color=vp.vector(0.58, 0.153, 0.68),
               mass=5.976 * (10 ** 24), momentum=vp.vector(0, 29.78 * 5.976 * (10 ** 24) / T, 0),
               make_trail=Tru)

p4 = vp.sphere(pos=vp.vector(206.7 * (10 ** 6), 0, 0), radius=5000000, color=vp.color.orange,
               mass=0.63345 * (10 ** 24), momentum=vp.vector(0, 24.13 * 0.63345 * (10 ** 24) / T, 0),
               make_trail=Tru)

p5 = vp.sphere(pos=vp.vector(740.7 * (10 ** 6), 0, 0), radius=15000000, color=vp.color.red,
               mass=1876.64328 * (10 ** 24), momentum=vp.vector(0, 13.07 * 1876.64328 * (10 ** 24) / T, 0),
               make_trail=Tru)

p6 = vp.sphere(pos=vp.vector(1.35 * (10 ** 9), 0, 0), radius=15000000, color=vp.color.blue,
               mass=561.80376 * (10 ** 24), momentum=vp.vector(0, 9.69 * 561.80376 * (10 ** 24) / T, 0),
               make_trail=Tru)

p7 = vp.sphere(pos=vp.vector(2.73 * (10 ** 9), 0, 0), radius=15000000, color=vp.color.white,
               mass=86.0544 * (10 ** 24), momentum=vp.vector(0, 6.81 * 86.0544 * (10 ** 24) / T, 0),
               make_trail=Tru)

p8 = vp.sphere(pos=vp.vector(4.45 * (10 ** 9), 0, 0), radius=15000000, color=vp.color.green,
               mass=101.592 * (10 ** 24), momentum=vp.vector(0, 5.43 * 101.592 * (10 ** 24) / T, 0),
               make_trail=Tru)


def F(x, y):
    G = 6.67 * (10 ** (-11))
    rVector = x.pos - y.pos
    rMagnitude = vp.mag(rVector)
    rHat = rVector / rMagnitude
    f = - rHat * G * x.mass * y.mass / rMagnitude ** 2
    return f


def K1(X, Y):
    a = (X.pos.x - Y.pos.x) ** 2
    b = (X.pos.y - Y.pos.y) ** 2
    c = math.sqrt(a + b)
    return c


def K2(X, Y):
    a = math.atan(abs(X.pos.y - Y.pos.y) / abs(X.pos.x - Y.pos.x))
    if X.pos.x > Y.pos.x:
        xx = Y.radius * math.cos(a) + Y.pos.x
    else:
        xx = X.radius * math.cos(a) + X.pos.x
    if X.pos.y > Y.pos.y:
        yy = Y.radius * math.sin(a) + Y.pos.y
    else:
        yy = X.radius * math.sin(a) + X.pos.y
    return xx, yy, Y.radius / 2


t = 0
dt = 0.01

while True:
    vp.rate(500)
    star.force = F(star, p1) + F(star, p2) + F(star, p3) + F(star, p4) + F(star, p5) + F(star, p6) + F(star, p7) + F(
        star, p8) + F(star, k1)
    p1.force = F(p1, star) + F(p1, p2) + F(p1, p3) + F(p1, p4) + F(p1, p5) + F(p1, p6) + F(p1, p7) + F(p1, p8) + F(p1,
                                                                                                                   k1)
    p2.force = F(p2, star) + F(p2, p1) + F(p2, p3) + F(p2, p4) + F(p2, p5) + F(p2, p6) + F(p2, p7) + F(p2, p8) + F(p2,
                                                                                                                   k1)
    p3.force = F(p3, star) + F(p3, p1) + F(p3, p2) + F(p3, p4) + F(p3, p5) + F(p3, p6) + F(p3, p7) + F(p3, p8) + F(p3,
                                                                                                                   k1)
    p4.force = F(p4, star) + F(p4, p1) + F(p4, p2) + F(p4, p3) + F(p4, p5) + F(p4, p6) + F(p4, p7) + F(p4, p8) + F(p4,
                                                                                                                   k1)
    p5.force = F(p5, star) + F(p5, p1) + F(p5, p2) + F(p5, p3) + F(p5, p4) + F(p5, p6) + F(p5, p7) + F(p5, p8) + F(p5,
                                                                                                                   k1)
    p6.force = F(p6, star) + F(p6, p1) + F(p6, p2) + F(p6, p3) + F(p6, p4) + F(p6, p5) + F(p6, p7) + F(p6, p8) + F(p6,
                                                                                                                   k1)
    p7.force = F(p7, star) + F(p7, p1) + F(p7, p2) + F(p7, p3) + F(p7, p4) + F(p7, p5) + F(p7, p6) + F(p7, p8) + F(p7,
                                                                                                                   k1)
    p8.force = F(p8, star) + F(p8, p1) + F(p8, p2) + F(p8, p3) + F(p8, p4) + F(p8, p5) + F(p8, p6) + F(p8, p7) + F(p8,
                                                                                                                   k1)
    k1.force = F(k1, star) + F(k1, p1) + F(k1, p2) + F(k1, p3) + F(k1, p4) + F(k1, p5) + F(k1, p6) + F(k1, p7) + F(k1,
                                                                                                                   p8)

    star.momentum = star.momentum + star.force
    p1.momentum = p1.momentum + p1.force * dt
    p2.momentum = p2.momentum + p2.force * dt
    p3.momentum = p3.momentum + p3.force * dt
    p4.momentum = p4.momentum + p4.force * dt
    p5.momentum = p5.momentum + p5.force * dt
    p6.momentum = p6.momentum + p6.force * dt
    p7.momentum = p7.momentum + p7.force * dt
    p8.momentum = p8.momentum + p8.force * dt
    k1.momentum = k1.momentum + k1.force * dt

    star.pos = star.pos + star.momentum / star.mass * dt
    p1.pos = p1.pos + p1.momentum / p1.mass * dt
    p2.pos = p2.pos + p2.momentum / p2.mass * dt
    p3.pos = p3.pos + p3.momentum / p3.mass * dt
    p4.pos = p4.pos + p4.momentum / p4.mass * dt
    p5.pos = p5.pos + p5.momentum / p5.mass * dt
    p6.pos = p6.pos + p6.momentum / p6.mass * dt
    p7.pos = p7.pos + p7.momentum / p7.mass * dt
    p8.pos = p8.pos + p8.momentum / p8.mass * dt
    k1.pos = k1.pos + k1.momentum / k1.mass * dt

    t += dt

    q += 1

    if q == 200:        # пунктир
        q = 0
        Tru = (Tru - 1) * -1
        k1.make_trail = Tru
        p1.make_trail = Tru
        p2.make_trail = Tru
        p3.make_trail = Tru
        p4.make_trail = Tru
        p5.make_trail = Tru
        p6.make_trail = Tru
        p7.make_trail = Tru
        p8.make_trail = Tru

    if K1(k1, star) <= k1.radius + star.radius:
        x, y, r = K2(k1, star)
        break
    if K1(k1, p1) <= k1.radius + p1.radius:
        x, y, r = K2(k1, p1)
        break
    if K1(k1, p2) <= k1.radius + p2.radius:
        x, y, r = K2(k1, p2)
        break
    if K1(k1, p3) <= k1.radius + p3.radius:
        x, y, r = K2(k1, p3)
        break
    if K1(k1, p4) <= k1.radius + p4.radius:
        x, y, r = K2(k1, p4)
        break
    if K1(k1, p5) <= k1.radius + p5.radius:
        x, y, r = K2(k1, p5)
        break
    if K1(k1, p6) <= k1.radius + p6.radius:
        x, y, r = K2(k1, p6)
        break
    if K1(k1, p7) <= k1.radius + p7.radius:
        x, y, r = K2(k1, p7)
        break
    if K1(k1, p8) <= k1.radius + p8.radius:
        x, y, r = K2(k1, p8)
        break

for i in range(100000, 10000000, 10000):
    vzr = vp.sphere(pos=vp.vector(x, y, 0), radius=r + i, color=vp.color.red,
                    mass=1, momentum=vp.vector(0, 0, 0))
    """a = random.randint(0, 90)
    l = random.randint(1, r + i)
    a1 = random.randint(0, 90)
    l1 = random.randint(1, r + i)
    a2 = random.randint(0, 90)
    l2 = random.randint(1, r + i)
    for j in range(10000, 100000, 5000):
        v = vp.sphere(pos=vp.vector(x + l * math.cos(a * math.pi / 2), y + l * math.sin(a * math.pi / 2), 0),
                      radius=r / 2 + j, color=vp.color.orange,
                      mass=1, momentum=vp.vector(0, 0, 0), make_trail=True)
        v1 = vp.sphere(pos=vp.vector(x + l * math.cos(a * math.pi / 2), y + l * math.sin(a * math.pi / 2), 0),
                       radius=r / 2 + j, color=vp.color.orange,
                       mass=1, momentum=vp.vector(0, 0, 0), make_trail=True)
        v2 = vp.sphere(pos=vp.vector(x + l * math.cos(a * math.pi / 2), y + l * math.sin(a * math.pi / 2), 0),
                       radius=r / 2 + j, color=vp.color.orange,
                       mass=1, momentum=vp.vector(0, 0, 0), make_trail=True)
        time.sleep(0.0000000001)"""
    time.sleep(0.01)
