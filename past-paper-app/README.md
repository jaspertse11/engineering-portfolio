# Past-paper practice app

**Personal project · 2026 – present · Core working, statistics and recommendations in development**

## Summary

A desktop app for revising from past papers one question at a time. A splitter cuts each full exam PDF into one PDF per question. The app then picks a question, times the attempt, opens the crib when I finish, and records my score, topic and remarks. I use it for my own revision.

**How it was built:** I built this app with AI assistance.

## What works today

- **Extraction (`src/extractor.py`, PyMuPDF):** scans each page for question headings such as `3 (short)` with a regular expression and writes each question's page range to its own PDF, named like `QP_2020_1P1_3_short.pdf`. I have used it on the first-year Engineering papers 1P1 to 1P4 from 2014 to 2025, which gives about 380 questions. In my use the splits have been accurate; I have not measured the failure rate.
- **App (`src/main.py`, Tkinter):** a question timer with start, pause and resume; a question selector filtered by paper, topic, question type and completion, with a count for each filter; a finish form for score, topic (searchable list) and remarks; and automatic restore of an unfinished question when the app restarts. On finishing it opens the crib PDF for that year if one is present.
- **Storage (`src/storage.py`):** one JSON file holds every question and its attempts (attempt number, score, time taken, remarks and timestamp).

## Not built yet

- **Statistics viewer:** the attempt history is recorded but there is no view of it yet.
- **Weak-topic recommendations:** `choose_next_question` in `src/app_logic.py` is the hook for this. At the moment it draws at random from the filtered pool.

## Known issues

- With no filter set, the next question is drawn from the whole pool, so it can be one I have already completed. Setting the selector to "Incomplete" avoids this.
- Opening PDFs uses `os.startfile`, so the app is Windows-only as written.
- I have not written automated tests. `src/prototype_splitter.py` is an earlier prototype of the splitter, kept for history.

## Layout and running it

Requires Python 3.10 or later.

```
pip install -r requirements.txt
```

Put the papers next to the code in this layout:

```
src/
  1P1/QP/QP_2020.pdf        question paper, one PDF per year
  1P1/CRIB/CRIB_2020.pdf    optional crib for the same year
  (likewise 1P2, 1P3, 1P4)
```

Then run:

```
cd src
python extractor.py   # creates QP_split/ with one PDF per question
python main.py        # starts the app
```

I do not publish the exam papers, which are copyrighted, or my own attempt data (`questions.json`). Both are in `.gitignore`.

## If I were to continue this project

- Build the statistics viewer, with performance over time and by topic.
- Implement the recommender, starting from a simple rule that favours topics where my recent scores are lowest or my last attempt is oldest, and check whether it improves my results.
- Test the extractor across different exam layouts and measure how often it fails.
- Make "next question" default to unattempted questions, and replace `os.startfile` so the app runs on other platforms.
