# Spider_clock
import turtle
import math
import time

# -----------------------------
# SCREEN
# -----------------------------
screen = turtle.Screen()
screen.title("Spider Clock")
screen.setup(800, 800)
screen.bgcolor("#7a3f05")
screen.tracer(0)

# -----------------------------
# DRAWING TURTLE
# -----------------------------
pen = turtle.Turtle()
pen.hideturtle()
pen.speed(0)

# -----------------------------
# DRAW WEB
# -----------------------------
def draw_web():

    pen.color("#d09a62")
    pen.pensize(1)

    # Straight web lines
    for angle in range(0, 360, 15):
        pen.penup()
        pen.goto(0, 0)
        pen.setheading(angle)
        pen.pendown()
        pen.forward(260)

    # Web circles
    for radius in range(40, 261, 40):
        pen.penup()
        pen.goto(0, -radius)
        pen.setheading(0)
        pen.pendown()
        pen.circle(radius)


# -----------------------------
# DRAW NUMBERS
# -----------------------------
def draw_numbers():

    numbers = turtle.Turtle()
    numbers.hideturtle()
    numbers.penup()
    numbers.color("white")

    for number in range(1, 13):

        angle = math.radians(number * 30)

        x = 290 * math.sin(angle)
        y = 290 * math.cos(angle)

        numbers.goto(x, y - 10)

        numbers.write(
            str(number),
            align="center",
            font=("Arial", 20, "bold")
        )


# -----------------------------
# DRAW SPIDER
# -----------------------------
def draw_spider():

    spider = turtle.Turtle()
    spider.hideturtle()
    spider.speed(0)
    spider.color("white")

    # Body
    spider.penup()
    spider.goto(0, -25)
    spider.pendown()
    spider.begin_fill()
    spider.circle(25)
    spider.end_fill()

    # Head
    spider.penup()
    spider.goto(0, 20)
    spider.pendown()
    spider.begin_fill()
    spider.circle(15)
    spider.end_fill()

    # Legs
    spider.pensize(4)

    legs = [
        (-10, 15, -70, 60),
        (-18, 5, -75, 20),
        (-20, -5, -75, -15),
        (-15, -18, -65, -55),

        (10, 15, 70, 60),
        (18, 5, 75, 20),
        (20, -5, 75, -15),
        (15, -18, 65, -55)
    ]

    for x1, y1, x2, y2 in legs:

        spider.penup()
        spider.goto(x1, y1)
        spider.pendown()
        spider.goto(x2, y2)


# -----------------------------
# DRAW CLOCK HAND
# -----------------------------
def draw_hand(angle, length, width):

    hand = turtle.Turtle()
    hand.hideturtle()
    hand.speed(0)
    hand.color("white")
    hand.pensize(width)

    hand.penup()
    hand.goto(0, 0)

    # 12 o'clock = 0 degrees
    hand.setheading(90 - angle)

    hand.pendown()
    hand.forward(length)


# -----------------------------
# CENTER DOT
# -----------------------------
def draw_center():

    center = turtle.Turtle()
    center.hideturtle()
    center.penup()
    center.goto(0, 0)
    center.color("red")
    center.dot(14)


# -----------------------------
# CLOCK UPDATE
# -----------------------------
def update_clock():

    # Clear previous web
    pen.clear()

    # Draw web
    draw_web()

    # Draw numbers
    draw_numbers()

    # Get current time
    current_time = time.localtime()

    hour = current_time.tm_hour % 12
    minute = current_time.tm_min
    second = current_time.tm_sec

    # Calculate angles
    hour_angle = (hour + minute / 60) * 30
    minute_angle = minute * 6
    second_angle = second * 6

    # Draw hands
    draw_hand(hour_angle, 120, 8)
    draw_hand(minute_angle, 180, 5)
    draw_hand(second_angle, 220, 2)

    # Draw spider
    draw_spider()

    # Draw center
    draw_center()

    # Update screen
    screen.update()

    # Repeat every second
    screen.ontimer(update_clock, 1000)


# -----------------------------
# TITLE
# -----------------------------
title = turtle.Turtle()
title.hideturtle()
title.penup()
title.goto(0, 335)
title.color("white")

title.write(
    "SPIDER CLOCK",
    align="center",
    font=("Arial", 28, "bold")
)


# -----------------------------
# START
# -----------------------------
print("Spider Clock is running...")

update_clock()

# Keep window open
screen.mainloop()
