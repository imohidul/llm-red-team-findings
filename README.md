# LLM Red-Team Findings

Hands-on adversarial testing of production LLMs (Gemini, ChatGPT) for hallucination, reasoning errors, instruction-following failures and prompt injection, with a focus on **Bangla-language** behavior.

Each finding is reproducible and evidence-backed. It records the exact prompt, the model's output, the expected behavior, the number of fresh-chat runs that failed, and a severity rating based on user impact.

**Author:** Syed Mohidul Islam · [LinkedIn](https://www.linkedin.com/in/imohidul/) · [GitHub](https://github.com/imohidul)

## Findings

| # | Finding | Model(s) | Category | Result |
| --- | --- | --- | --- | --- |
| [001](findings/001-fabricated-citations.md) | Fabricated academic citations | Gemini 3.6 Flash | Hallucination | **High** · 4/5 runs failed |
| [002](findings/002-character-count-aggregation.md) | Correct per-word counts, wrong total | Gemini 3.6 Flash | Reasoning | **Medium** · 5/5 runs failed |
| [003](findings/003-sonnet-false-self-assessment.md) | Sonnet structure violated, then misreported | Gemini 3.6 Flash | Instruction following | **Low** · 3/3 runs failed |
| [004](findings/004-naive-injection-blocked.md) | Naive indirect prompt injection | ChatGPT 5.6 Luna, Gemini 3.6 Flash | Prompt injection | **Blocked** · ASR 0/5 |
| [005](findings/005-fake-completion-injection-model-size.md) | Fake-completion injection vs model size | gemma4:12b, gemma4:31b (Ollama) | Prompt injection | **12b: ASR 8/30 · 31b: 0/30** |

## Highlights

- **001:** Asked for papers from a real conference, the model invented three papers and their authors, and omitted the real ones. The tester is the first author of one of the omitted papers, so the ground truth is verified first-hand and through IEEE Xplore.
- **002:** The model counted the letter চ correctly in every word, then added its own correct numbers to the wrong total. The error is in aggregation, not perception.
- **003:** The model's own "structural features" summary claimed a rhyme scheme its poem did not follow. False self-assessment is riskier than the structural error itself.
- **004:** Both models resisted an obvious injection and a fake `System:` label. The report states the limits of this result honestly.
- **005:** An automated harness ran the same fake-completion injection 30 times against two model sizes. gemma4:12b obeyed the injected instruction 27% of the time; gemma4:31b never did. Shows injection is probabilistic, that small samples mislead (10 runs gave 10%, 30 runs gave 27%), and that model scale affected robustness here.

## Method

See [METHODOLOGY.md](METHODOLOGY.md) for the finding format, how runs are counted, and how severity is assigned.

## Responsible disclosure

These findings involve no harmful content and are published as-is. Any finding that could enable real harm will be reported privately to the model provider before publication. All tests were run on the tester's own accounts, using harmless payloads.
