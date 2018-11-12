---
title: "Rejection Letter"
date: 2018-11-11T16:12:12-05:00
showDate: false
draft: false
tags: [science, interview, industry]
---

# Currency Converter

I currently received a rejection letter of my number one choice company after an over the phone interview.
I felt pretty bummed about the entire situation so I decided to write out a complete solution for the technical question I was asked. You can check out the Github [here](https://github.com/ChiefMateStarbuck/Currency)

## The problem

Given a .txt list of conversion rates stylized like so:

```
USD,JPY,1.1
USD,IMP,.8
IMP,RUB,8.2
JMD,USD,.6

EXAMPLE:
[FIRST CURRENCY], [SECOND CURRENCY], [CONVERSION RATE]
USD,JPY,1.1

5 Dollars * 1.1 = 5.5 Yen
```

Create a function that converts, one currency to another.

### Challenge

Your function must be able to determine a path in order to convert the beginning currency to the last. In our example above the path in order to convert USD to RUB is:

```
USD->IMP->RUB
```

## My Solution

First things first, I load the .txt file into a map:

```python


file = open('exchange.txt', 'r')

load = {}

for line in file:
    line_list =  line.split(',')
    line_list[2] = line_list[2][:-1]

    if line_list[0] not in load:
        # The first element in the map is a list which will hold the conversion rates 
        # of the current path. (I will be implementing a Breadth First Search to  find the path)
        load[line_list[0]] = [[]]

    """
    The map will look like this:

    'JMD': [[],['RUB',8.2],['USD,.6]]
    'RUB':[[], ['JPY',1.9]]
    etc ..

    """
    load[line_list[0]].append([line_list[1], float(line_list[2])])
```

Next, we calculate:

```python


def exchange(load, begin, end, amount):
    fringe = []
    visited = []
    fringe.append(load[begin][1:])

    while fringe:
        top = fringe.pop(0)

        if top[0] != end:
            visited.append(top[0])
            temp_children = load[top[0]][1:]

            for child in temp_children:

                if child[0] == end:
                    conversion = []
                    conversion.extend(load[top[0]][0])
                    conversion.append(top[1])
                    conversion.append(child[1])

                    for con in conversion:
                        amount *= con
                    return con

                if child[0] not in visited and child[0] in load: 
                    fringe.append(child)
                    load[child[0]][0].extend(load[top[0]][0])
                    load[child[0]][0].append(top[1])
                    load[child[0]][1] = top[0]

    return -1

```

## How To Use

1. Download the GitHub and cd into the folder
2. (Optional) Add your own values to the 'exchange.txt' file
3. Run the engine like so: `python3 engine.py [Beginning Currency] [End Currency] [amount]`

Example: `python3 engine.py USD RUB 93.21`

That's it! You're ready to go.