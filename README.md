#include <stdio.h>
#include <stdlib.h> // For rand(), srand()
#include <time.h>   // For time() (to seed the random number generator)

// Function prototypes
void display_rules();
int get_lottery_bet();
int get_coin_bet();
void play_game(int lotto_bet, int coin_bet);

int main() {
    // 1. Seed the random number generator
    // This ensures different results each time the program runs.
    srand(time(NULL)); 
    
    int player_lotto_bet, player_coin_bet;
    char play_again = 'y';
    
    // Display game rules
    display_rules();

    // Main game loop
    while (play_again == 'y' || play_again == 'Y') {
        
        // 2. Get player's bets
        player_lotto_bet = get_lottery_bet();
        player_coin_bet = get_coin_bet();
        
        // 3. Play the game and check results
        play_game(player_lotto_bet, player_coin_bet);
        
        // 4. Ask the player if they want to play again
        printf("\nDo you want to play again? (y/n): ");
        if (scanf(" %c", &play_again) != 1) {
            // Handle simple input error
            break;
        }
    }
    
    printf("\nThank you for playing! Goodbye.\n");
    
    return 0;
}

// Function to display the game rules and prize structure
void display_rules() {
    printf("==========================================\n");
    printf("   🪙 COIN TOSS & LOTTERY GAME RULES 🪙   \n");
    printf("==========================================\n");
    printf("1. Choose a Lottery Number (1-10) and a Coin Side (1 for Heads, 0 for Tails).\n");
    printf("2. The game will randomly generate both results.\n");
    printf("3. Prizes:\n");
    printf("   - Match Coin AND Lottery Number:   Grand Prize! (e.g., 500 points)\n");
    printf("   - Match ONLY Lottery Number:       Medium Prize! (e.g., 100 points)\n");
    printf("   - Match ONLY Coin Side:            Small Prize! (e.g., 50 points)\n");
    printf("==========================================\n\n");
}

// Function to get the player's lottery number bet
int get_lottery_bet() {
    int bet;
    while (1) {
        printf("Enter your Lottery Number bet (1-10): ");
        if (scanf("%d", &bet) == 1 && bet >= 1 && bet <= 10) {
            return bet; // Valid input
        } else {
            printf("Invalid input. Please enter a number between 1 and 10.\n");
            // Clear the input buffer on failure
            while (getchar() != '\n');
        }
    }
}

// Function to get the player's coin side bet
int get_coin_bet() {
    int bet;
    while (1) {
        printf("Enter your Coin Side bet (1 for Heads, 0 for Tails): ");
        if (scanf("%d", &bet) == 1 && (bet == 0 || bet == 1)) {
            return bet; // Valid input
        } else {
            printf("Invalid input. Please enter 1 (Heads) or 0 (Tails).\n");
            // Clear the input buffer on failure
            while (getchar() != '\n');
        }
    }
}

// Function to run the toss and check results
void play_game(int lotto_bet, int coin_bet) {
    
    // Generate random results
    // rand() % 10 + 1 generates a number between 1 and 10
    int actual_lotto = rand() % 10 + 1; 
    
    // rand() % 2 generates 0 or 1
    int actual_coin = rand() % 2; 
    
    // Display results
    printf("\n--- Results ---\n");
    printf("Your Lottery Bet: %d\n", lotto_bet);
    printf("Actual Lottery Number: %d\n", actual_lotto);
    printf("Your Coin Bet: %s\n", (coin_bet == 1 ? "Heads" : "Tails"));
    printf("Actual Coin Toss: %s\n", (actual_coin == 1 ? "Heads" : "Tails"));
    printf("---------------\n");

    // Check for a win condition
    int lotto_match = (lotto_bet == actual_lotto);
    int coin_match = (coin_bet == actual_coin);

    if (lotto_match && coin_match) {
        printf("🎉 GRAND PRIZE! You matched both the Lottery Number and the Coin Toss!\n");
    } else if (lotto_match) {
        printf("💰 MEDIUM PRIZE! You matched the Lottery Number!\n");
    } else if (coin_match) {
        printf("🪙 SMALL PRIZE! You matched the Coin Toss!\n");
    } else {
        printf("Sorry, you didn't match any results. Better luck next time!\n");
    }
}
