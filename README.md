# Final-exam
### Python Journal Template
### PART 1: Defining Your Problem  
**Task:** State the problem you are planning to solve.  

**Problem Statement:**  
Develop a simulation program for the casino dice game Craps to help users understand their odds of winning/losing. The program will:  
1. **Input Data:**  
   - Number of games to simulate (user input)  
   - Random dice rolls generated during gameplay  

2. **Program Functionality:**  
   - Play a sample game demonstrating rules  
   - Simulate multiple games based on user input  
   - Track win/loss statistics and average rolls per outcome  

3. **Outputs:**  
   - Real-time game results during simulation  
   - Summary statistics (wins/losses, average rolls)  
   - Log file containing session details  

**Inspiration Questions Answered:**  
- Solved problem: Visualizing probability in a gambling context  
- Data sources: Random dice rolls, user input for game count  
- User interaction: Initial setup + optional continuous play  
- End product: Statistical insights + educational gameplay demonstration  

---

### PART 2: Working Through Specific Examples  
**Task:** Outline steps for simplified scenarios.  

**Scenario 1: Immediate Win on First Roll**  
1. Roll two dice → values (3,4) → sum 7  
2. Check sum against winning conditions (7/11)  
3. Output "You win!"  

**Scenario 2: Loss After Multiple Rolls**  
1. Initial roll (1,4) → sum 5 → continue  
2. Next roll (3,6) → sum 9 → continue  
3. Subsequent roll (4,3) → sum 7 → loss  
4. Output "You lose!"  

**Scenario 3: Edge Case - Continuous Rolls**  
1. Initial sum 4 → continue  
2. Rolls cycle: (2,3)=5 → (1,5)=6 → (6,1)=7  
3. Output "You lose after 3 rolls"  

---START PROGRAM
    DISPLAY "Welcome to the Craps Simulation Tool!"
    
    // Run a sample game to demonstrate rules
    CALL playSampleGame()
    
    // Prompt user for number of simulations
    DISPLAY "Enter the number of games to simulate:"
    SET userInput TO getInput()
    VALIDATE userInput IS POSITIVE INTEGER
    SET numberOfGames TO INTEGER(userInput)
    
    // Initialize statistical counters
    SET totalWins TO 0
    SET totalLosses TO 0
    SET totalRollsForWins TO 0
    SET totalRollsForLosses TO 0
    
    // Execute multiple games
    FOR EACH game IN 1..numberOfGames:
        CREATE newPlayerInstance
        SET gameOutcome TO newPlayerInstance.playGame()
        
        IF gameOutcome == "WIN":
            INCREMENT totalWins BY 1
            ADD newPlayerInstance.rollCount TO totalRollsForWins
        ELSE:
            INCREMENT totalLosses BY 1
            ADD newPlayerInstance.rollCount TO totalRollsForLosses
    
    // Calculate and display statistics
    DISPLAY "Simulation Results:"
    DISPLAY "Total Wins:", totalWins
    DISPLAY "Total Losses:", totalLosses
    IF totalWins > 0:
        COMPUTE avgRollsPerWin = totalRollsForWins / totalWins
        DISPLAY "Average Rolls per Win:", avgRollsPerWin
    IF totalLosses > 0:
        COMPUTE avgRollsPerLoss = totalRollsForLosses / totalLosses
        DISPLAY "Average Rolls per Loss:", avgRollsPerLoss
    DISPLAY "Winning Percentage:", (totalWins / numberOfGames) * 100, "%"
    
    // Save results to log file
    CALL saveLog(totalWins, totalLosses, avgRollsPerWin, avgRollsPerLoss)
END PROGRAM
---

### PART 4: Testing Your Program  
**Testing Process & Fixes:**  
1. **Test Case 1:** Invalid Input Handling  
   - **Error:** Entered "abc" → ValueError  
   - **Fix:** Added `try/except` block with input validation loop  
   - **Result:** Program now prompts repeatedly until valid integer entered  

2. **Test Case 2:** Edge Case - Continuous Rolls  
   - **Issue:** Incorrectly tracking roll counts  
   - **Fix:** Implemented `getNumberOfRolls()` method in Player class  
   - **Verification:** Manual calculation matched program output  

3. **Test Case 3:** Multi-Game Statistics  
   - **Observation:** Average rolls displayed incorrectly  
   - **Fix:** Initialized counters inside game loop  
   - **Result:** Correct averages calculated per outcome type  

---

### PART 5: Commenting Your Program  
**Code Comments Excerpt:**  
```python
# player.py - Simulates player actions in Craps game

class Player:
    def __init__(self):
        # Initialize two six-sided dice
        self.first_die = Die()
        self.second_die = Die()
        # Track game state variables
        self.start_of_game = True
        self.winner = False
        self.loser = False
        self.roll_count = 0

    def roll_dice(self):
        """Simulate rolling both dice and update game state"""
        self.roll_count += 1  # Increment roll counter
        self.first_die.roll()
        self.second_die.roll()
        # Determine current roll outcome
        current_sum = self.first_die.value + self.second_die.value
        if self.start_of_game:
            self.handle_initial_roll(current_sum)
        else:
            self.handle_subsequent_roll(current_sum)

### PART 3: Generalizing Into Pseudocode  
**Pseudocode Structure:**  
