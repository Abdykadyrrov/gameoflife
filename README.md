# gameoflife
This game is based on the game of life. It shows how humanity multiplies. 
The blue cells are male individuals, and the pink ones are female, respectively.

In the game, 4 cells of random color are randomly created, then the program reads the cells and stops when it finds blue cells, after which the program reads the cells located in the neighboring blue cells for the presence of pink cells. When the blue and pink cells touch, 4 more random-colored cells are generated in neighboring cells, and the old cells die. The game continues as long as the cells remain, let's say, without their pair, thereby ending the kind of life.
Project









from tkinter import *
import random


class Field:
    def __init__(self, c, n, m, width, height, walls=False):
        ''' 
       c - canvas instance 
       n - number of rows 
       m - number of columns 
       width - width of game field in pixels 
       height - width of game field in pixels 
       walls - if True matrix should have 0's surrounded by 1's (walls) 
       example 
       1 1 1 1 
       1 0 0 1 
       1 1 1 1 
       '''
        self.c = c
        self.a = []
        self.n = n + 2
        self.m = m + 2
        self.width = width
        self.height = height
        self.count = 0
        for i in range(self.n):
            self.a.append([])
            for j in range(self.m): 
                self.a[i].append(0)

        self.a[1 + 10][2 + 10] = 1
        self.a[2 + 10][2 + 10] = 2
      
        self.draw()

    def step(self):
        b = []
        for i in range(self.n):
            b.append([])
            for j in range(self.m):
                b[i].append(0)

        for i in range(1, self.n - 1):
            for j in range(1, self.m - 1):
                if self.a[i][j] == 1:
                    neib_sum = self.a[i - 1][j - 1] + self.a[i - 1][j] + self.a[i - 1][j + 1] + self.a[i][j - 1] + \
                               self.a[i][j + 1] + self.a[i + 1][j - 1] + self.a[i + 1][j] + self.a[i + 1][j + 1]
                    if neib_sum <= 1 or neib_sum > 8:
                        b[i][j] = 0
                    elif 2 <= neib_sum <= 8:
                        b[i + random.randint(-1, 1)][j + random.randint(-1, 1)] = random.randint(1, 2)
                        b[i + random.randint(-1, 1)][j + random.randint(-1, 1)] = random.randint(1, 2)
                        b[i + random.randint(-1, 1)][j + random.randint(-1, 1)] = random.randint(1, 2)
                        b[i + random.randint(-1, 1)][j + random.randint(-1, 1)] = random.randint(1, 2)

        self.a = b

    def print_field(self):
        for i in range(self.n):
            for j in range(self.m):
                print(self.a[i][j], end="")
            print()

    def draw(self):
        ''' 
        draw each element of matrix as a rectangle with white background and wall rectangle should have dark grey background 
        '''
        color = "grey"
        sizen = self.width // (self.n - 2)
        sizem = self.height // (self.m - 2)
        for i in range(1, self.n - 1):
            for j in range(1, self.m - 1):
                if self.a[i][j] == 1:
                    color = "blue"
                elif self.a[i][j] == 2:
                    color = "pink"
                else:
                    color = "white"
                self.c.create_rectangle((i - 1) * sizen, (j - 1) * sizem, (i) * sizen, (j) * sizem, fill=color)
        self.step()
        self.c.after(1, self.draw)


root = Tk()
root.geometry("400x400")
c = Canvas(root, width=400, height=400)
c.pack()

f = Field(c, 30, 30, 400, 400)

root.mainloop()
f.print_field()



![gameoflife](https://user-images.githubusercontent.com/103096307/162630380-2e63894c-ae3e-4bb1-9cca-18dd292a665b.PNG)
