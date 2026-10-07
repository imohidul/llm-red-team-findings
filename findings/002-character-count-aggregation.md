# Finding #002 — Correct per-word counts, wrong total

- **Model:** Gemini 3.6 Flash · **Tested:** 2026-10-07 · **Language:** Bangla prompt
- **Tester:** Syed Mohidul Islam
- **Category:** Reasoning (character counting / arithmetic aggregation)

## Prompt

> চাচি চা চায় চাচা চ্যচায় চাচা চাচইর চ্যাচামেচিতে চামিলি চমকে চায় বাক্যে কয়টি চ আছে।

*(How many instances of the letter চ are in this sentence?)* The count is verified on the exact prompt string as sent, including its misspellings.

## Model output (final answers across 5 runs)

19, 17, 17, 17, 17

## Expected behavior

Answer **18**. Verified programmatically with `str.count('চ')` on the exact sentence.

## Why this is a failure

- The model failed to give the correct total in every run.
- In its own working (appendix), every per-word count is correct, and the numbers it lists sum to 18. It then states the sum is 17. The error is in adding up its own correct intermediate steps, not in reading the letters.
- The wrong total is stated confidently, with no sign of uncertainty.

## Reproducibility

5/5 fresh chats failed (one run answered 19, four answered 17).

## Severity

**Medium.** Counting letters in a tongue-twister has low direct impact. However, the model reaches a wrong conclusion from correct intermediate steps and presents it confidently. The same failure could affect totals in bills, invoices or data summaries, where users may trust the final number without checking the steps.

## Appendix — model working (one run)

> আসুন বাক্যটি শব্দভিত্তিক ভেঙে 'চ' অক্ষরের সংখ্যা নিখুঁতভাবে হিসেব করা যাক:
>
> 1. চাচি — ২ টি
> 2. চা — ১ টি
> 3. চায় — ১ টি
> 4. চাচা — ২ টি
> 5. চ্যচায় — ২ টি
> 6. চাচা — ২ টি
> 7. চাচইর — ২ টি
> 8. চ্যাচামেচিতে — ৩ টি
> 9. চামিলি — ১ টি
> 10. চমকে — ১ টি
> 11. চায় — ১ টি
>
> সংখ্যাগুলো পরপর যোগ করলে পাই: 2 + 1 + 1 + 2 + 2 + 2 + 2 + 3 + 1 + 1 + 1 = 17

(Correct sum of the listed numbers: 18.)
