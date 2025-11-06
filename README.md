#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <ctype.h> // For toupper()

// Function to generate the computer's choice (1=Snake, 2=Water, 3=Gun)
int generateRandomChoice() {
    // Seed the random number generator using current time
    srand(time(0));
    
    // Generate a random number between 1 and 3 (inclusive)
    return (rand() % 3) + 1;
}

// Function to convert the choice number to a display string
const char* choiceToString(int choice) {
    if (choice == 1) return "Snake";
    if (choice == 2) return "Water";
    if (choice == 3) return "Gun";
    return "Invalid";
}

// Function to determine the winner
// Returns: 0 for Tie, 1 for Player Win, -1 for Computer Win
int determineWinner(int playerChoice, int computerChoice) {
    // 0: Tie
    if (playerChoice == computerChoice) {
        return 0;
    }
    
    // Player Wins (Winning combinations: Snake vs Water, Water vs Gun, Gun vs Snake)
    // (1 vs 2) -> Snake drinks Water
    // (2 vs 3) -> Water rusts Gun
    // (3 vs 1) -> Gun shoots Snake
    if ((playerChoice == 1 && computerChoice == 2) || 
        (playerChoice == 2 && computerChoice == 3) || 
        (playerChoice == 3 && computerChoice == 1)) {
        return 1; // Player wins
    } 
    
    // All other cases are Computer Win
    return -1; // Computer wins
}

int main() {
    int playerChoice = 0;
    int computerChoice;
    char inputChar;
    int result;

    printf("--- Snake, Water, Gun Game ---\n");
    printf("Enter your choice:\n");
    printf(" S - Snake\n W - Water\n G - Gun\n");
    
    // Input loop to ensure a valid choice is entered
    while (playerChoice == 0) {
        printf("Your move (S/W/G): ");
        if (scanf(" %c", &inputChar) != 1) { // Space before %c skips previous newline
            // Clear input buffer on error
            while (getchar() != '\n');
            continue;
        }

        // Convert input character to the corresponding integer choice
        switch (toupper(inputChar)) {
            case 'S':
                playerChoice = 1; // 1 = Snake
                break;
            case 'W':
                playerChoice = 2; // 2 = Water
                break;
            case 'G':
                playerChoice = 3; // 3 = Gun
                break;
            default:
                printf("Invalid input. Please enter S, W, or G.\n");
        }
    }

    // 1. Generate computer's choice
    computerChoice = generateRandomChoice();

    // 2. Display choices
    printf("\nPlayer chose: %s\n", choiceToString(playerChoice));
    printf("Computer chose: %s\n", choiceToString(computerChoice));

    // 3. Determine and display result
    result = determineWinner(playerChoice, computerChoice);

    printf("\n*** Result ***\n");
    if (result == 1) {
        printf("🎉 You WIN! 🎉\n");
    } else if (result == -1) {
        printf("💻 Computer WINS! Try again. 😞\n");
    } else {
        printf("🤝 It's a TIE! 🤝\n");
    }

    printf("---------------\n");
    
    return 0;
}
