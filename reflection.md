# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").
1. When I first ran the game, the UI loaded fine but the hints were completely backward
2. The count was always off by one.
3. The New Game button was broken. It never resets.

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?  Copilot

**Correct suggestion:**
The AI correctly identified that the New Game button was broken because `st.session_state.status` was never reset back to `"playing"` after a win or loss. It suggested adding `st.session_state.status = "playing"` inside the new game handler. I verified this by winning a game and then clicking New Game — before the fix the screen stayed frozen with the "You already won" message, and after the fix the game restarted properly and accepted new guesses.

**Incorrect or misleading suggestion:**
The AI initially suggested fixing the hint messages only in the `try` block of `check_guess`, but left the `except TypeError` fallback path with the old backwards messages still in place. That meant on even-numbered attempts — when the secret was secretly converted to a string and triggered the `TypeError` path — the hints were still wrong. I caught this by reading the full function carefully and noticing the fallback block was untouched. The real fix was to remove the string-conversion logic entirely so the `TypeError` path could never be reached.

---

## 3. Debugging and testing your fixes

I decided a bug was really fixed only when I could see the correct behavior both in the live game and in a passing pytest test — passing the test alone wasn't enough because some bugs only show up when Streamlit reruns the whole script. For the hint bug, I added two pytest cases: one that checks `check_guess(60, 50)` returns `"Too High"` and another that verifies the message contains `"LOWER"` — before the fix that second test failed, which confirmed the bug was real and the fix was targeted. I also manually played through a full game on each difficulty after every fix to make sure nothing broke the UI. The AI helped me write the test structure and suggested unpacking the tuple return value (`outcome, _ = check_guess(...)`) which I didn't think to do at first since the original starter tests were written incorrectly comparing the tuple directly to a string.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- What change did you make that finally gave the game a stable secret number?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
