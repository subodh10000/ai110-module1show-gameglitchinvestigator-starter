# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

The game was a number-guessing game with a sidebar for difficulty, a text input for guesses, and Developer Debug Info showing the secret. **Bug 1 (State Bug):** The secret number in the debug panel changed every time you clicked Submit—it never stayed the same, so you couldn't win even when you knew the answer. **Bug 2 (Hints):** When the guess was too high, it said "Go HIGHER!" instead of "Go LOWER!", and vice versa. **Bug 3 (Score):** The score logic was inconsistent—it added or subtracted points in odd ways based on attempt parity, and didn't reliably reflect wins vs. losses.

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

I used Cursor (with AI chat and Agent mode) to fix the bugs and refactor. **Correct suggestion:** The AI identified that the secret was being converted to a string on every other attempt (`if st.session_state.attempts % 2 == 0: secret = str(...)`), which broke comparisons. Removing that and always using the int secret fixed the state bug—verified by running the app and seeing the secret stay the same in Debug Info until New Game. **Incorrect/misleading:** None in this session; the AI’s fixes were accurate and tests passed.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

I checked fixes by running the app and by running `pytest tests/ -v`. **pytest:** `test_guess_too_high` and `test_guess_too_low` confirm that `check_guess(60, 50)` returns "Too High" and `check_guess(40, 50)` returns "Too Low". All three tests passed after the refactor. The AI updated the tests to unpack the `(outcome, message)` tuple from `check_guess` so they match the new return format.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

Streamlit reruns the whole script from top to bottom every time you interact (click a button, change a widget). Normal variables are recreated each run, so they reset. That’s why the secret number kept changing. `st.session_state` is a special dictionary that survives between reruns. If you store values there (e.g. `st.session_state.secret`), they stay the same until you change or delete them. For a game, you need session state for persistent data like the secret number and score.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

**Reuse:** Run `pytest` after every change. It’s fast and catches regressions quickly. **Do differently:** Run the app manually before and after fixes to confirm behavior, not just rely on unit tests. **Takeaway:** AI-generated code can be close to correct but still have subtle bugs (e.g. wrong hints, state bugs). Treat it as a starting point and always verify with tests and manual checks.
