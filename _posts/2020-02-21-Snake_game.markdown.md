---
layout: post
title:  "Design A Snake Game"
date:   2020-02-21 14:36:23
category: problems
---

<span class="image featured"><img src="/images/snake_game.jpg" alt=""></span>
This is a [leetcode problem](https://leetcode.com/problems/design-snake-game/).

A better displayed version can be found [here](https://github.com/Chandler-Qian/chandler-qian.github.io/blob/master/_posts/2020-02-21-Snake_game.markdown).

## Problem Discription



Design a  [Snake game](https://en.wikipedia.org/wiki/Snake_(video_game))  that is played on a device with screen size =  _width_  x  _height_.  [Play the game online](http://patorjk.com/games/snake/)  if you are not familiar with the game.

The snake is initially positioned at the top left corner (0,0) with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

## My Solution
View my posted solution [here](https://leetcode.com/problems/design-snake-game/discuss/508190/java-solution-using-deque-without-set).

To maintain the shape of the snake, we can use a deque to record all positions the snake occupies, from head to tail. It's obvious that the head of the snake is the first element in the queue and the tail of the snake is the last. The reason why we use string to represent the position is that if we use array, it's harder for us to check if one position is inside the snake's body.  

In each move, we check if we go outside the border, we hit the food or we hit ourself. Then we remove the tail, add a new head accordingly.
```
class SnakeGame {
    private Deque<String> snake;
    private int width, height;
    private Queue<int[]> foods;
    private Map<String, int[]> dirMap;
    /** Initialize your data structure here.
        @param width - screen width
        @param height - screen height 
        @param food - A list of food positions
        E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */
    public SnakeGame(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        snake = new ArrayDeque<>();
        snake.addLast("0,0");
        foods = new LinkedList<>();
        for (int[] f: food) foods.offer(f);
        dirMap = new HashMap<>();
        dirMap.put("U", new int[]{-1, 0});
        dirMap.put("L", new int[]{0, -1});
        dirMap.put("R", new int[]{0, 1});
        dirMap.put("D", new int[]{1, 0});
    }
    
    /** Moves the snake.
        @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down 
        @return The game's score after the move. Return -1 if game over. 
        Game over when snake crosses the screen boundary or bites its body. */
    public int move(String direction) {
        // retrieve the head of the snake
        String curPos = snake.peekFirst();
        // retrieve next potential move
        int[] nextMove = dirMap.get(direction);
        // get the position of next move
        int nextRow = Integer.valueOf(curPos.split(",")[0]) + nextMove[0];
        int nextCol = Integer.valueOf(curPos.split(",")[1]) + nextMove[1];
        // get next possible food
        int[] food = foods.peek();
        // check if next move goes outside the bound
        if (nextRow < 0 || nextRow >= height || nextCol < 0 || nextCol >= width) return -1;
        // check if next move eats the food
        if (food != null && nextRow == food[0] && nextCol == food[1]) {
            snake.addFirst(nextRow + "," + nextCol);
            foods.poll();
            return snake.size() -1;
        }
        // else, remove the tail
        snake.pollLast();
        // and check if next move bites itself
        if (snake.contains(nextRow + "," + nextCol)) return -1;
        else {
            // if not, add a new head to the queue
            snake.addFirst(nextRow + "," + nextCol);
            return snake.size() - 1;
        }
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
```