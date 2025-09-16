#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Declarations of the two functions you will implement
// Feel free to declare any helper functions or global variables
void printPuzzle(char **arr);
void searchPuzzle(char **arr, char *word);
int bSize;

// Main function, DO NOT MODIFY
int main(int argc, char **argv)
{
    if (argc != 2)
    {
        fprintf(stderr, "Usage: %s <puzzle file name>\n", argv[0]);
        return 2;
    }
    int i, j;
    FILE *fptr;

    // Open file for reading puzzle
    fptr = fopen(argv[1], "r");
    if (fptr == NULL)
    {
        printf("Cannot Open Puzzle File!\n");
        return 0;
    }

    // Read the size of the puzzle block
    fscanf(fptr, "%d\n", &bSize);

    // Allocate space for the puzzle block and the word to be searched
    char **block = (char **)malloc(bSize * sizeof(char *));
    char *word = (char *)malloc(20 * sizeof(char));

    // Read puzzle block into 2D arrays
    for (i = 0; i < bSize; i++)
    {
        *(block + i) = (char *)malloc(bSize * sizeof(char));
        for (j = 0; j < bSize - 1; ++j)
        {
            fscanf(fptr, "%c ", *(block + i) + j);
        }
        fscanf(fptr, "%c \n", *(block + i) + j);
    }
    fclose(fptr);

    printf("Enter the word to search: ");
    scanf("%s", word);

    // Print out original puzzle grid
    printf("\nPrinting puzzle before search:\n");
    printPuzzle(block);

    // Call searchPuzzle to the word in the puzzle
    searchPuzzle(block, word);

    return 0;
}

// Places the found position of path in front of the previous, ex: 42
int printPath(int num, int digit)
{
    int temp = num;
    int power = 1;

    while (temp > 0)
    {
        power *= 10;
        temp /= 10;
    }

    return digit * power + num;
}

void printPuzzle(char **arr)
{
    // This function will print out the complete puzzle grid (arr).
    // It must produce the output in the SAME format as the samples
    // in the instructions.
    // Your implementation here...

    for (int i = 0; i < bSize; i++)
    {
        for (int j = 0; j < bSize; j++)
        {
            printf("%c ", *(*(arr + i) + j));
        }

        printf("\n");
    }
}

int search(char **arr, char *word, int *path, int x, int y, int position)
{
    int previous = *(path + x * bSize + y);

    // Add position to the front of previous position
    *(path + x * bSize + y) = printPath(*(path + x * bSize + y), position);

    // Word is found in relation to size of position
    if (*(word + (position)) == '\0')
    {
        return 1; // Successfully found the word
    }

    // Iterate to top, middle, and bottom row
    for (int i = -1; i < 2; i++)
    {
        // Iterate through left, middle, and right column
        for (int j = -1; j < 2; j++)
        {
            if (j == 0 && i == 0)
            {
                continue; // if at current char skip the current iteration of the loop and jumps to next
            }

            int newX = x + j;
            int newY = y + i;

            // Checks to see if it is in bounds
            if (newX >= 0 && newX < bSize && newY >= 0 && newY < bSize)
            {
                // If next letter of word is in the found premises
                if (*(*(arr + newX) + newY) == *(word + position))
                {
                    //  Call the function itself and continue searching next letter of the word from the new position
                    if (search(arr, word, path, newX, newY, position + 1))
                    {
                        return 1;
                    }
                }
            }
        }
    }

    *(path + x * bSize + y) = previous;

    return 0;
}

// Converting inputed word to all capital using ASCII
void upper(char *str)
{
    while (*str)
    {
        if (*str >= 'a' && *str <= 'z')
        {
            *str = *str - 32;
        }
        str++;
    }
}

void searchPuzzle(char **arr, char *word)
{
    // This function checks if arr contains the search word. If the
    // word appears in arr, it will print out a message and the path
    // as shown in the sample runs. If not found, it will print a
    // different message as shown in the sample runs.
    // Your implementation here...

    upper(word);
    int wordExists = 0;

    // Clears it and sets it to 0 using calloc
    int *path = (int *)calloc(bSize * bSize, sizeof(int));

    for (int i = 0; i < bSize; i++)
    {
        for (int j = 0; j < bSize; j++)
        {
            if (*(*(arr + i) + j) == *(word))
            {
                // printf("\nStarting search from (%d, %d) matching '%c'\n", i, j, *word);
                if (search(arr, word, path, i, j, 1))
                {
                    wordExists = 1;
                }
            }
        }
    }
    if (wordExists)
    {
        printf("\nWord Found!\n");
        printf("Printing the search path: \n");
        for (int i = 0; i < bSize; i++)
        {
            for (int j = 0; j < bSize; j++)
            {
                printf("%d\t", *(path + i * bSize + j));
            }
            printf("\n");
        }
    }
    else
    {
        printf("\nWord not found!\n");
    }
    free(path);
}
