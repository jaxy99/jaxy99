SCREEN 12
WIDTH 40, 25
COLOR 2, 0 ' Green snake on a black background

DIM snakeX(100), snakeY(100)
snakeLength = 3 ' Initial length of the snake
direction = 1 ' 1: Right, 2: Down, 3: Left, 4: Up

' Initialize snake position
FOR i = 1 TO snakeLength
    snakeX(i) = i
    snakeY(i) = 1
NEXT i

' Place the first food item
foodX = INT(RND * 40) + 1
foodY = INT(RND * 25) + 1

DO
    ' Draw snake
    FOR i = 1 TO snakeLength
        PSET (snakeX(i), snakeY(i)), 1
    NEXT i

    ' Draw food
    PSET (foodX, foodY), 4 ' Yellow color for food

    ' Move snake
    INPUT "Press Enter to continue", action$
    IF INKEY$ = CHR$(13) THEN
        snakeLength = snakeLength + 1
        FOR i = snakeLength TO 2 STEP -1
            snakeX(i) = snakeX(i - 1)
            snakeY(i) = snakeY(i - 1)
        NEXT i

        ' Update snake head position based on direction
        IF direction = 1 THEN
            snakeX(1) = snakeX(1) + 1
        ELSEIF direction = 2 THEN
            snakeY(1) = snakeY(1) + 1
        ELSEIF direction = 3 THEN
            snakeX(1) = snakeX(1) - 1
        ELSEIF direction = 4 THEN
            snakeY(1) = snakeY(1) - 1
        END IF

        ' Check for collision with food
        IF snakeX(1) = foodX AND snakeY(1) = foodY THEN
            ' Place new food item
            foodX = INT(RND * 40) + 1
            foodY = INT(RND * 25) + 1
        ELSE
            ' Erase tail of the snake
            PSET (snakeX(snakeLength), snakeY(snakeLength)), 0
        END IF

        ' Check for collision with walls or itself
        IF snakeX(1) < 1 OR snakeX(1) > 40 OR snakeY(1) < 1 OR snakeY(1) > 25 THEN
            PRINT "Game Over! You hit a wall."
            SLEEP
            SYSTEM
        END IF

        FOR i = 2 TO snakeLength
            IF snakeX(1) = snakeX(i) AND snakeY(1) = snakeY(i) THEN
                PRINT "Game Over! You collided with yourself."
                SLEEP
                SYSTEM
            END IF
        NEXT i
    END IF

LOOP UNTIL FALSE
