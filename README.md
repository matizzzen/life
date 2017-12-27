# life
game of life
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as ani
import random


def survive(x, y):
    global universe
    count = int(np.sum(universe[x-1:x+2, y-1:y+2]) - universe[x, y])
    if universe[x, y] == 1:
        if count < 2 or count > 4:
            new_universe[x, y] = 0
    elif universe[x, y] == 0:
        if count == 3:
            new_universe[x, y] = 1


def conf_out(x):
    with open(x+'_conf.txt', 'w') as last_config:
        last_config.write(str(widt)+' '+str(hidt))


def conf_in(x):
    global widt, hidt
    with open(x+'_conf.txt', 'r') as cfg:
        X = cfg.read()
        X = X.split()
        widt = int(X[0])
        hidt = int(X[1])


def pattern_in(x):
    global pattern
    with open(x+'.txt', 'r') as file:
        temp = [row.strip() for row in file]
        for i in range(hidt):
            temp[i] = list(map(int, temp[i]))
        pattern = np.array(temp)


def pattern_out(x):
    with open(x+'.txt', 'w') as file:
        for i in range(hidt):
            for j in range(widt):
                file.write(str(int(pattern[i][j])))
            file.write('\n')


def default(x):
    plt.imshow(x, cmap='binary')


def nextgen():
    global universe
    for i1 in range(universe.shape[0]):
        for j1 in range(universe.shape[1]):
            survive(i1, j1)
    universe = np.copy(new_universe)


choise = int(input('input:\nfor input - 1\nfor random - 2\nfor saved - 3\n>> '))

if choise == 1:
    widt = int(input('input width >> '))
    hidt = int(input('input height >> '))
    pattern = [[int(j) for j in input()] for i in range(hidt)]
elif choise == 2:
    widt = int(input('input width >> '))
    hidt = int(input('input height >> '))
    pattern = np.zeros((hidt, widt))
    for i in range(hidt):
        for j in range(widt):
            pattern[i][j] = random.randint(0, 1)
else:
    name = input('input name >> ')
    conf_in(name)
    pattern_in(name)

universe = np.zeros((100, 100))
xos = int((100-hidt)/2)
yos = int((100-widt)/2)
universe[xos:xos+hidt, yos :yos+widt] = pattern
new_universe = np.copy(universe)
ims = []
fig = plt.figure()
plt.axis('off')
much = int(input('input count (1...10) >> '))
much *= 100
speed = int(input('input speed(1...10) >> '))
speed = int(1000/speed)
nachal = np.copy(universe)
for k in range(much):
    ims.append((plt.imshow(universe, cmap='binary'),))
    nextgen()
anime = ani.ArtistAnimation(fig, ims, interval=speed)
#default(nachal)
plt.show()
if choise != 3:
    safe = input('Save pattern? (y/n)\n>> ')
    if safe == 'y':
        name = input('input name >> ')
        pattern_out(name)
        conf_out(name)
    else:
        exit()
else:
    exit()
