# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

I ran the game and saw a number-guessing UI with a sidebar for difficulty and Developer Debug Info. I noticed three bugs: (1) The secret number in the debug panel changed every time I clicked Submit, so I couldn't win even when I knew the answer. (2) The hints were reversed—when my guess was too high it said "Go HIGHER!" instead of "Go LOWER!" (3) The score changed in odd ways and didn't seem to reflect wins and losses correctly.

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

I used Cursor (AI chat and Agent mode) as a companion. I asked it to fix the state bug and to move the logic into `logic_utils.py`. When it suggested removing the alternating int/str conversion and always using `st.session_state.secret` as an int, I ran the app and confirmed the secret stayed the same until I clicked New Game. I also asked it to fix the Higher/Lower hints; it swapped them, and I verified by playing a few rounds. I didn't get any incorrect suggestions this time, but I still ran the app and pytest before accepting the changes.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

I decided a bug was fixed by running the app and by running `pytest tests/ -v`. I played the game manually to see if the secret stayed the same and if the hints were correct. I ran pytest to confirm `check_guess` behaved correctly—e.g., `check_guess(60, 50)` returns "Too High" and `check_guess(40, 50)` returns "Too Low." The AI updated the tests to unpack the tuple from `check_guess`, and I ran them to make sure they passed.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

Streamlit reruns the whole script every time you interact with the app. Normal variables get recreated each run, so they reset. `st.session_state` is a dictionary that survives between reruns, so values like the secret number and score persist until you change them. I learned this from the AI's explanation and from seeing how the state bug behaved before and after the fix.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

I want to keep running `pytest` after changes to catch regressions quickly. Next time, I'd try to understand the AI's suggestions more deeply before accepting them, instead of just applying fixes. This project showed me that AI-generated code can look right but still have subtle bugs, so I should always verify with tests and manual checks instead of trusting it blindly.