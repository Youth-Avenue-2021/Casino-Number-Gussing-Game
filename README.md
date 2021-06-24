
# Casino-Number-Gussing-Game

Add description of game
## Documentation

### How to play this game !

- Choose a number between 1 to 100
- Winner gets 20 times of the bid money
- Wrong choice, and you lose (bid amount / 10)$ per wrong guess of the amount you bet.
- Wrong choice till end the game, you loss multiplied by 2 of bid amount from total balance
- Number of chance to guess the number is 10.

To quit the game anytime, press ctrl+c
## Online Compile

Compile this game in online plateform

- [Compile Here](https://replit.com/@AbhiDadhaniya/Casino-Game#main.cpp)

  
## Code of this game

```c++
#include <iostream>
// below two libraries are to generate random number 
#include <cstdlib>
#include <ctime>
using namespace std;

// function prototype
void getRules();
int getAmountData();
int gameLogic();

// global variables
string userName;
int totalAmount = 0;
int betAmount = 0;

// main function
int main(void) {

    getRules();   

    // take player's name
    cout << "\nPlease enter your name : ";
    cin >> userName;

    // following do-while loop will run until the balance is zero
    do {
        // the following if condition runs iff total amount is zero provided it runs once in the entire game
        if (totalAmount == 0) {
            getAmountData();
            gameLogic();
        }
        else {
            string playAgain;   
            cout << "\nDO you want to play again ? (y/n) : ";
            cin >> playAgain;

            // if balance is 0, the loop terminates i.e., game ends
            if (totalAmount > 0) {
                    if (playAgain == "y" || playAgain == "Y") {
                    // display current balance
                    cout << "Now, Your total balance is : " << totalAmount << "$" << endl;
                    gameLogic();   // re-starts the game
                }

                // if no, terminate the do-while loop
                else if (playAgain == "n" || playAgain == "N" || totalAmount == 0) {
                    cout << "Thank you for playing this game !\n" << endl;
                    break;
                }

                // if player enters inappropriate response
                else {
                    cout << "Please enter proper characters." << endl;
                    cout << "\nDO you want to play again ? (y/n) : ";
                    cin >> playAgain;
                }
            }
            else {
                cout << "\n Ooppss! Your balance is vanished, you cannot play with zero balance." << endl;
                cout << "Thank you for playing this game !\n" << endl;
                break;
            }
        }
    } while (totalAmount != 0);

    return 0;
}

// function to take & display player's balance 
int getAmountData() {
    cout << "\nEnter your balance : $";
    cin >> totalAmount;
    cout << "Your total balance is: " << totalAmount << "$" << endl;
    return 0;
}

// function describing the game logic
int gameLogic() {
    // to declare the bid amount
    int betAmountScope = totalAmount - 10;
    // to declare the loss per wrong guess
    int decreasingAmount;
    // the following do-while loop checks two conditions:
    // 1. bid amount is less than or equal to balance
    // 2. bid amount is multiple of 10

    do {
        // loop to check condition 1
        do {
            if (totalAmount < betAmountScope) {
                cout << "\nYour betting amount should be less than your balance, please bid again: $";
                cin >> betAmountScope;
            }
        } while (totalAmount < betAmountScope);
        // loop to check condition 2
        do {
            if (betAmountScope % 10 != 0) {
                cout << "Please enter correct bid amount by required (multiplication by 10)...!" << endl;
            }
            cout << "\nPlease enter your bid amount (In *10) : $";
            cin >> betAmountScope;
        } while (betAmountScope % 10 != 0);
    } while (totalAmount < betAmountScope || betAmountScope % 10 != 0);

    cout << "you bet " << betAmountScope << "$" << endl;

    // decrease the bidding amount from total balance for the first time if player loses
    totalAmount -= betAmountScope;
    cout << "\nNow, Your total balance is : " << totalAmount << "$" << endl;
    cout << "you have total 10 maximum chances to guess correct number...!" << endl;

    // betamount stores the bid amount i.e., 20-18-16-14-12
    // betamountscope stores the bidding amount
    betAmount = betAmountScope;

    // money decreasing logic:
    // if user bids 10$ then 1$ cuts per wrong guess
    // if user bids 20$ then 2$ cuts per wrong guess
    // if user bids 30$ then 3$ cuts per wrong guess
    // and so on...
    decreasingAmount = (betAmountScope / 10);
    cout << "Decrease " << decreasingAmount << "$ per wrong guess...!" << endl;

    // generate a random number
    srand(time(0));
    // limit of random number between : 1 => 100
    int randomNumber = (rand() % 100) + 1;
    // cout << "Random No is : " << randomNumber << endl;

    // local scope variable for player to guess number
    int userChoice;
    cout << "\nHey " << userName << "! Guess a number between 1 to 100 : ";
    cin >> userChoice;

    // this do-while loop terminates once player wins by guessing correct number
    do {
        // hint for the player to guess the number
        if (randomNumber < userChoice) {
            // hint if guessed number is 10 numbers behind/forword
            if (userChoice - randomNumber < 10) {
                cout << "good! you are too close please try to guess the correct number...!" << endl;
            }

            // if wrong guess, decrease n$ from betting amount
            betAmount -= decreasingAmount;
            cout << "Now your bet amount is : $" << betAmount << endl;
            cout << "Please guess lower number...!" << endl;
            cout << "\nguess again : ";
            cin >> userChoice;
        }

        // hint for the player to guess the number
        else if (randomNumber > userChoice) {
            if (randomNumber - userChoice < 10) {
                cout << "good! you are too close please try to guess the correct number...!" << endl;
            }
            betAmount -= decreasingAmount;
            cout << "Now your bet amount is : $" << betAmount << endl;
            cout << "Please guess higher number..!" << endl;
            cout << "\nguess again : ";
            cin >> userChoice;
        }

        // if bid amount vanished, loop breaks and player loses the game
        if (betAmount <= 1) {
            cout << "Oops! You lost the game...! Your betting amount finished (your chances finished...!)" << endl;
            // decrease betamount from balance (2nd time... because by rule bid amount*2 cut from total balance)
            totalAmount -= betAmountScope;
            if (totalAmount < 0) {
                cout << "Now, Your total balance is : 0$" << endl;
            }
            else {
                cout << "Now, Your total balance is : " << totalAmount << "$" << endl;
            }
            break;
        }
    } while (userChoice != randomNumber);

    // if correctly guessed, player wins
    if (userChoice == randomNumber) {
        cout << "Congratulations " << userName << ", You won the game...!" << endl;
        // by rule, user gets 20 times of the bid amount...
        totalAmount += betAmount * 20;
        cout << "Now, Your total balance is : " << totalAmount << "$" << endl;
    }
    return 0;
}

// function to display rules of the game
void getRules() {
    // command to clear terminal...
    system("cls");
    // it works only for few online terminals/compiler
    
    cout << "\n\t= = = = = = = = = = = = = = = = = = = = = = = =\n";
    cout << "\n\t     * CASINO NUMBER GUESSING RULES! * \n";
    cout << "\n\t-----------------------------------------------\n";
    cout << endl;
    cout << "\t1. Choose a number between 1 to 100\n";
    cout << "\t2. Winner gets 20 times of the bid money\n";
    cout << "\t3. Wrong choice, and you lose (bid amount / 10)$ per wrong guess of the amount you bet\n";
    cout << "\t4. Wrong choice till end the game, you lose twice the bid amount from total balance\n";
    cout << "\t5. Number of chances to guess the number is 10.\n";
    cout << "\t==> To quit the game, press ctrl+c\n";
    cout << "\n\t= = = = = = = = = = = = = = = = = = = = = = = =\n";
    cout << endl;
}

```

  
## Contributing

Contributions are always welcome!
  