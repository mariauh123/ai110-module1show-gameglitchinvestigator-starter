# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

  I originally thought the "submit guess" button was not working, I had to press twice for my guess to actually submit. This meant that every submitted guess was one behind my current guess, the list only displayed every other guess if I did not double submit each guess. This is the 'submit' variable in app.py

  The hints were backwards, and even encouraged out of bound guesses since they kept hinting to "go lower/go higher" even if the previous guess was 0/100 respectively. The code controlling this behavior currently lives in app.py under the check_guess function.

  "New Game" button is not working as intended, although a new secret number is generated and attempts reset, the game does not actually start over. Once the game is over (whether it's out of attempts or secret was guessed) it does not allow the user to submit new guesses depite clicking "New Game". I had to manually refresh the page to play more than once. This logic lives inside app.py under the variable name new_game.

  "Guess a number between _ and _" is not displaying correctly, it always shows "between 1 and 100" despite the difficulty selected and the range that comes with it (app.py, st.info).

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

|     Input  | Expected Behavior  | Actual Behavior  |   Console Output / Error    | Suspected Code Location |
|------------|--------------------|------------------|------------------------------|-------------------------|
|   guess 15 | 'Go Higher' hint   | 'Go Lower' hint  |     none                     | app.py,check_guess      |
|  'new game'| Allow guesses      | Unable to guess  |'Start new game to try again' | app.py, new_game        |
|guess 1...14|Display all guesses | odd guess only   |History just shows odd guesses| app.py, submit          | 
|Difficulty|Hard: Highest range|Hard: middle range|Hard: 1-50, but Normal: 1-100|app.py, get_range_for_difficulty|

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

I used Gemini, we targeted the "Submit Guess" button behavior. Gemini explained that the reason why every guess wasn't captured is because Streamlit was rerunning the script when the text input changed AND when submit was clicked, the text was updated AFTER the submission was ran, causing previous submissions to be added to history, and the in-between submissions to never be recorded at all. Gemini suggested to wrap the raw-guess and submit variables in a form. This meant that submit was no longer side-by-side with the "New Game" and "Show hint". The fix did capture every attempt but history still only updated 1 attempt behind (guess 1, then guess 2 -> On submit of 2, 1 appears in history), and not all guess are shown at the end (Normal allows 8 guesses, but only 7 are shown in history). Gemini then explained that history is delayed due to how Streamlit renders the page, the widget is getting the old information to render since new guess is appended after the list is rendered. We fixed this by moving the "Developer Debug Info" to after the guess is made and appended. Finally, in order to get all our attempts, I changed the initial value of session_state.attempts from 1 to 0. And gemini also gave me a freebie on how to fix the "Guess a number between _ and _" banner with the correct range (use the variables low and high).
---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
