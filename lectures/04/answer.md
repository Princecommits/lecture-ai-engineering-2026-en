# Lecture 04 Homework

## Homework

- Level attempted: Advanced
- Benchmark: [SWE-bench Verified](https://huggingface.co/datasets/SWE-bench/SWE-bench_Verified)
- What I did: I reviewed five task summaries in the [dataset viewer](https://huggingface.co/datasets/SWE-bench/SWE-bench_Verified/viewer/default/test) and checked the [official repository](https://github.com/SWE-bench/SWE-bench), [dataset guide](https://github.com/SWE-bench/SWE-bench/blob/main/docs/guides/datasets.md), and [OpenAI report](https://openai.com/index/introducing-swe-bench-verified/).

### Problems I read

I read the issue titles and summaries:

1. [astropy #12907](https://github.com/astropy/astropy/issues/12907): nested `CompoundModel` composition gives the wrong separability matrix. I’d trace how models combine and check the tests. The tricky part for an LLM is understanding the intended math, not just changing the output; I’d also check that independent inputs remain independent.
2. [astropy #13033](https://github.com/astropy/astropy/issues/13033): a missing required column causes a misleading `TimeSeries` exception. This seems approachable if the validation path and expected behavior are clear.
3. [astropy #13236](https://github.com/astropy/astropy/issues/13236): asks whether structured columns should become `NdarrayMixin` objects automatically. This feels gnarlier because it could affect the API and existing callers; the tricky part is weighing the change against compatibility.
4. [astropy #13398](https://github.com/astropy/astropy/issues/13398): proposes a direct ITRS-to-observed coordinate transformation. I’d need astronomy knowledge and careful numerical tests; coordinate-frame conventions and accuracy seem like the hard parts.
5. [astropy #13453](https://github.com/astropy/astropy/issues/13453): HTML output for ASCII tables ignores formatting options. This seems contained, but it’s easy to miss format-specific edge cases or break another output format. Tests should cover other formats and existing styles too.

I didn’t check out the repositories or try patches, so these are first impressions, not claims that I solved the tasks. The main challenge for an LLM is understanding project behavior and domain rules, then changing code without causing regressions.

### How it was built and graded

The [OpenAI report](https://openai.com/index/introducing-swe-bench-verified/) says 93 Python-experienced developers screened SWE-bench. It analyzes 1,699 random samples, each independently labeled by three annotators; the most severe label was kept. OpenAI engineers hand-labeled 50 examples for training, and annotators had to pass onboarding tests.

The annotations rate issue clarity and test validity from 0 to 3: 0–1 are minor concerns, while 2–3 indicate a sample should be excluded.

Tasks were excluded for vague issues, FAIL_TO_PASS tests that might reject valid solutions, or other major problems. A severity score of 2 or 3 from any annotator was enough. The report says 68.3% of samples were filtered for these issues. The authors prioritized 1–4 hour and over-4-hour tasks, then randomly sampled the rest to make the 500-task Verified set. The 1,794-task difference from the full benchmark (2,294) is not necessarily the number individually reviewed and rejected.

Tasks pair real GitHub issues with repository snapshots. An agent writes a patch, which is applied in Docker and checked with repository tests, per the [evaluation guide](https://github.com/SWE-bench/SWE-bench/blob/main/docs/guides/evaluation.md). FAIL_TO_PASS tests fail before the fix and pass afterward; PASS_TO_PASS tests should pass both before and after. Grading is test-based, not an LLM judgment.

I read the report provided with this homework and checked the dataset counts, sample issues, and grading method against public sources.

### Comparison with the lecture evaluation set

SWE-bench Verified has 500 coding tasks; our lecture set has 10 questions. Its screening involved 93 Python-experienced developers and three labels per annotated sample, compared with one instructor for our set. SWE-bench grades patches with repository tests; our set uses human and LLM judgments against expected behavior. Its tasks resemble real maintenance work, while ours don’t require changing a full codebase.

- Observations/questions: Test-based grading is concrete and repeatable, but passing doesn’t guarantee a clean or fully correct patch. Human and LLM grading can assess qualities tests miss, though results may be less consistent.
