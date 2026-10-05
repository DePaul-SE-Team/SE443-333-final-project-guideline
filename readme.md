# Final Project: Benchmarking AI Test Generation with SWT-Bench

## 1. Objective

Build and evaluate a method that generates regression tests from real-world software issue descriptions. Compare your method with a baseline, submit reproducible results to the class leaderboard, and investigate what your results reveal about test quality.

The central research question is:

**Can your method generate tests that reproduce reported bugs more effectively or efficiently than a simple baseline?**

Your grade depends on experimental quality, reproducibility, and analysis; not leaderboard position. However, those who get the best result may receive bonus!

## 2. Preparation

Before beginning, complete the SWT-Bench paper review and read the benchmark’s [evaluation and submission documentation](https://github.com/logic-star-ai/SWT-Bench).

SWT-Bench evaluates generated tests against an original repository and its reference bug fix. Successful tests must expose the bug before the fix and pass afterward. The paper also evaluates how thoroughly generated tests exercise code changed by the fix. [Paper, Sections 3.1–3.3](https://arxiv.org/html/2406.12952v3)

By the end of this project, you should be able to:

- Run a repository-level test-generation benchmark.
- Design a controlled comparison between two methods.
- Interpret benchmark metrics alongside individual test cases.
- Package results so another person can reproduce them.
- Explain the limits of claims based on leaderboard scores.

## 3. Project Scope

Work individually or in a team of two to three students.

Each team must implement and evaluate:

1. **A baseline:** A simple, documented approach that prompts an AI model to generate tests from an issue description and repository context.
2. **An experimental method:** One deliberate change intended to improve the baseline.

Possible changes include:

- Retrieving more relevant source files or existing tests.
- Providing examples of repository testing conventions.
- Letting an agent execute and revise tests on the buggy repository.
- Generating multiple candidates and selecting one using observable evidence.
- Improving assertions or adding issue-specific edge cases.
- Reducing generation cost while preserving effectiveness.

Choose one main research question. A carefully evaluated small change is sufficient; training a new model is not required.

## 4. Dataset and Experimental Rules

### Shared evaluation set

The instructor will publish:

- A development set of approximately **20 instances**.
- A fixed evaluation set of approximately **50 instances**, drawn from SWT-Bench Lite and spanning multiple repositories.
- The dataset revision, instance IDs, evaluation-harness version, and resource limits.

Every team must evaluate both methods on the same evaluation instances. Class-subset scores must be labeled clearly and must not be presented as full SWT-Bench Lite results.

If resources require a smaller set, the instructor will revise the shared set before final evaluation.

### Fair comparison

Keep the model, repository access, and generation budget constant unless one of these is the variable you are studying. Record any unavoidable differences.

Before final evaluation, freeze your prompts, configuration, candidate-selection rule, and method version. Do not tune your method using final evaluation outcomes.

During generation, the method may access the issue description, original repository, existing tests, and permitted development tools. It must not access the reference fix, reference test patch, or evaluator-only artifacts.

Additional requirements:

- Produce test patches without modifying production behavior.
- Do not disable existing tests or weaken assertions to obtain a passing result.
- Select candidates without consulting reference-fix outcomes.
- Preserve all attempts and document retries.
- Keep missing predictions, invalid outputs, and generation timeouts in the evaluation denominator.
- Report infrastructure failures separately. Any exclusions must follow an instructor-defined rule applied consistently to all teams.

## 5. Evaluation and Class Leaderboard

Use the official evaluation harness, pinned to the instructor’s selected version.

Report these metrics:

| Metric | Required interpretation |
|---|---|
| **Issue reproduction success rate** | Percentage of evaluation instances with at least one generated test that fails before the reference fix, while all generated tests pass afterward. |
| **Change coverage** | Harness-reported coverage contribution on executable code affected by the reference fix. Explain the harness’s aggregation and any excluded cases. |
| **Patch applicability** | Percentage of generated test patches that apply successfully. |
| **Cost and runtime** | Total generation cost, average generation time per instance, and evaluation time, reported separately. |

Success rate captures bug reproduction; coverage captures additional exercise of relevant code. Neither alone establishes that a test fully represents the issue or that a patch is correct. [Paper, Section 3.3](https://arxiv.org/html/2406.12952v3)

Submit one baseline row and one experimental-method row:

| Team | Method/version | Dataset/instance count | Successes / total (%) | Change coverage | Applicability | Generation cost | Avg. generation time |
|---|---|---|---|---|---|---|---|

The class leaderboard will rank methods by success rate. Tied methods share a rank; cost and coverage remain visible for comparison.

For analysis:

- Report the paired difference between your methods.
- Count instances solved by both, only the baseline, only your method, and neither.
- Include a 95% confidence interval for the success-rate difference, using paired resampling over instances.
- Discuss uncertainty and repository composition when interpreting small differences.

For stochastic methods, run three repetitions if the shared budget permits. Otherwise, report the single-run limitation explicitly.

## 6. Required Test-Quality Analysis

Inspect at least **six instances** selected using a documented rule:

- Two successful reproductions.
- Two unsuccessful attempts.
- Two cases where the methods disagree.

If a category has too few cases, inspect all available cases and explain the substitution.

For each case, identify:

1. The behavior described by the issue.
2. The generated test’s input and assertion.
3. Its behavior before and after the reference fix.
4. Whether the observed failure meaningfully represents the issue.
5. What the case reveals about your method.

Discuss at least one threat to validity and one way to investigate it. Examples include weak assertions, flaky tests, benchmark contamination, narrow repository coverage, and tests that detect only one particular implementation of a fix.

## 7. Milestones

| Milestone | Suggested timing | Deliverable |
|---|---|---|
| Proposal and pilot | Week 1 | One-page research question, baseline, proposed change, budget, and three-instance pilot. |
| Development checkpoint | Week 2 | Working methods, development-set results, and preliminary failure analysis. |
| Method freeze | Week 3 | Frozen configuration, instance manifest, and reproducible evaluation procedure. |
| Final submission | Week 4 | Report, code, prediction artifacts, leaderboard rows, and presentation. |

The pilot must demonstrate that you can generate a test patch, evaluate it, and interpret the resulting logs.

## 8. Final Submission

Submit a repository or ZIP archive containing:

- **Report:** Approximately 1,500–2,000 words in Markdown or PDF.
- **Code and README:** Installation instructions and commands for generation, evaluation, and result aggregation.
- **Prediction artifacts:** Baseline and experimental-method JSONL files compatible with the selected harness.
- **Results:** Per-instance outcomes, aggregate metrics, cost records, and leaderboard rows.
- **Evidence:** Generated patches, evaluation logs, and available agent traces.
- **Presentation:** A five-minute explanation of your question, method, result, and strongest limitation.

Organize the report under these headings:

1. Research Question and Motivation
2. Methods and Experimental Design
3. Results and Leaderboard Comparison
4. Test-Quality and Failure Analysis
5. Threats to Validity and Conclusions
6. Reproducibility, Team Contributions, and AI Disclosure

Include names, complete references, model identifiers, access dates, prompts, settings, dependencies, dataset revision, and harness commit.

Disclose AI tools used for test generation, implementation, analysis, and writing. Identify which outputs your team checked.

## 9. Optional Official Leaderboard Submission

Teams may extend their evaluation to a complete supported SWT-Bench Lite or Verified split.

The benchmark repository currently requests prediction JSONL, local performance results, project and trace links, and reproduction information for official submissions. A class subset does not meet its complete-split requirement. [Official submission instructions](https://github.com/logic-star-ai/SWT-Bench#submitting-results-to-the-leaderboard)

Official submission is optional and earns no advantage based on acceptance or processing time.

## 10. Grading — 100 Points

| Criterion | Points |
|---|---:|
| Clear research question and justified experimental change | 15 |
| Working baseline and experimental method | 20 |
| Fair evaluation and correct metric reporting | 20 |
| Reproducible artifacts and leaderboard submission | 20 |
| Meaningful test-quality analysis and validity discussion | 15 |
| Clear report, presentation, contributions, and AI disclosure | 10 |
| **Total** | **100** |

A method that does not improve the baseline can earn full credit when the experiment is sound and the analysis explains the result.

## Reference

Mündler, N., Müller, M. N., He, J., & Vechev, M. (2024). *SWT-Bench: Testing and Validating Real-World Bug-Fixes with Code Agents*. Advances in Neural Information Processing Systems 37. https://doi.org/10.48550/arXiv.2406.12952

