---
title: turtle example turtle-pong (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtle-pong'


Modules used in program: 
* `import time`

## python turtle-pong

Python turtle example: turtle-pong

```python
# Copyright 2018 Dmytro Meleshko
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Пинг Понг с использованием черепашьей графики.
# ВНИМАНИЕ!!! Работает только на 3-ей версии Python!
# В этой один игрок играет против компьютера. Ракетка игрока управляется
# стрелками на клавиатуре. На клавишу escape можно выйти из игры. Есть куча
# багов, нету счета, но игра работает без крашей и перебоев, и подходит как
# proof of concept для моей задумки. Все объекты в игре (мячик и ракетки)
# являются черепахами, что сильно упрощает рендеринг. Также, у каждого класса,
# который описывает игровой объект, есть статический метод `get_shape` (который
# возвращает форму этого объекта, которая используется как спрайт или модель) и
# метод update (который принимает время, прошедшее с момента предыдущего кадра, и
# обновляет объект каждый кадр). Все скорости - это пиксели в секунду.

import time
from turtle import Turtle, Screen
from math import sin, cos, pi, copysign


class Ball(Turtle):
    RADIUS = 10
    SPEED = 600  # горизонтальная скорость, не меняется во время игры
    SHAPE_POINTS = 10  # количество точек, из которых состоит круг

    @staticmethod
    def get_shape():
        r = Ball.RADIUS
        points = map(
            lambda angle: (cos(angle) * r, sin(angle) * r),
            [i / Ball.SHAPE_POINTS * 2 * pi for i in range(0, Ball.SHAPE_POINTS)],
        )

        return tuple(points)

    def __init__(self):
        super().__init__(shape="ball")
        self.speed("fastest")
        self.penup()

        # вертикальная и горизонтальная скорость мяча
        self.vx = Ball.SPEED
        self.vy = 0

    def update(self, dt):
        r = Ball.RADIUS
        # следующие координаты мяча
        x = self.xcor() + self.vx * dt
        y = self.ycor() + self.vy * dt

        w, h = self.getscreen().screensize()

        # если мяч на следующем кадре вылазит за край экрана, то разворачиваем его
        if x - r <= -w or x + r >= w:
            self.vx = -self.vx
        if y - r <= -h or y + r >= h:
            self.vy = -self.vy

        self.goto(x, y)


# поведение ракеток игрока и компьютера имеет много общего, так что общий код
# вынесен в этот класс
class Racket(Turtle):
    WIDTH = 15
    HEIGHT = 60
    # скорость ракеток компьютера и игрока одинаковая, чтобы все было честно
    MOVEMENT_SPEED = 500
    MAX_BALL_BOUNCE = 900

    @staticmethod
    def get_shape():
        w, h = Racket.WIDTH, Racket.HEIGHT
        return ((w / 2, -h / 2), (w / 2, h / 2), (-w / 2, h / 2), (-w / 2, -h / 2))

    # конструктор ракетки получает x-коодинату (так как она не меняется) и ссылку
    # на мяч (чтобы обрабатывать столкновения)
    def __init__(self, x, ball):
        super().__init__(shape="racket")
        self.speed("fastest")
        self.penup()
        self.left(90)
        self.setx(x)
        self.ball = ball

    def update(self, dt):
        px, py = self.pos()
        pw, ph = Racket.WIDTH, Racket.HEIGHT

        br = Ball.RADIUS
        bx, by = self.ball.pos()

        # обработка столкновений с мячём
        if abs(px - bx) <= pw + br and abs(py - by) <= ph / 2:
            self.ball.vx = -self.ball.vx
            # изменяем вертикальную скорость мяча в зависимости от расстояния до
            # центра ракетки
            self.ball.vy = Racket.MAX_BALL_BOUNCE * abs(py - by) / (ph / 2)


# ракетка компьютера движется к y-координате мяча с постоянной скоростью
class BotRacket(Racket):
    def __init__(self, x, ball):
        super().__init__(x, ball)

    def update(self, dt):
        super().update(dt)
        # супер-хитрый способ получить направление к мячу (1 - вверх, -1 - вниз)
        direction = copysign(1, self.ball.ycor() - self.ycor())
        self.sety(self.ycor() + direction * Racket.MOVEMENT_SPEED * dt)


# ракетка игрока управляется стрелочками на клавиатуре
class PlayerRacket(Racket):
    def __init__(self, x, ball):
        super().__init__(x, ball)

        # направление, в котором движется ракетка (1 - вверх, -1 - вниз)
        self.direction = 0

        # так как лямбды не могут присваивать переменные, чтобы не городить кучу
        # методов я просто объявил эту функцию, которая изменяет направление
        def set_direction(direction):
            self.direction = direction

        # прикручиваем все слушатели событий
        screen = self.getscreen()
        screen.onkeypress(lambda: set_direction(1), "Up")
        screen.onkeyrelease(lambda: set_direction(0), "Up")
        screen.onkeypress(lambda: set_direction(-1), "Down")
        screen.onkeyrelease(lambda: set_direction(0), "Down")

    def update(self, dt):
        super().update(dt)
        self.sety(self.ycor() + self.direction * Racket.MOVEMENT_SPEED * dt)


# главный класс игры потому, что так красиво
class Pong:
    def __init__(self):
        self.running = False
        self.last_update_time = time.time()

        self.screen = Screen()

        # настраиваем экран
        w, h = self.screen.screensize()
        self.screen.setup(width=w * 2, height=h * 2)
        self.screen.delay(0)  # выключаем задержки между действиями черепашек
        self.screen.tracer(False)  # отключаем рендеринг для ускорения обновления
        self.screen.onkey(lambda: self.stop(), "Escape")

        # добавляем фигуры объектов
        self.screen.register_shape("ball", Ball.get_shape())
        self.screen.register_shape("racket", Racket.get_shape())

        # инициализируем объекты
        self.ball = Ball()
        self.bot = BotRacket(-w + Racket.WIDTH * 4, self.ball)
        self.player = PlayerRacket(w - Racket.WIDTH * 4, self.ball)

    def start(self):
        self.running = True
        self.screen.listen()
        self.update()
        self.screen.mainloop()

    # Этот метод просит модуль turtle вызвать метод update как можно скорее, тем
    # самым обеспечивая постоянное обновление кадров
    def schedule_update(self):
        self.screen.ontimer(self.update, 0)

    def update(self):
        if self.running:
            current_time = time.time()
            delta_time = current_time - self.last_update_time
            self.last_update_time = current_time

            for obj in [self.ball, self.bot, self.player]:
                obj.update(delta_time)

            self.screen.update()  # рендерим все за раз
            self.schedule_update()

    def stop(self):
        self.running = False
        self.screen.bye()


# Погнали!
Pong().start()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
