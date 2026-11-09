# Lecture 03 Answers

## Exercises

- Differences between the Lecture 01 (bare LLM) and agent answers:
  - In Lecture 01, the model answers from its general knowledge without checking the course materials.
  - In the agent version, the model uses tools to look up the glossary and lecture notes before answering, which makes the response more grounded in this course.
- Tool calls shown by `--trace` (tool names and arguments):
  - `search_glossary(query="LoRA")` when the user asks about a specific term.
  - `get_lecture_summary(lecture=3)` when the question is about the content of a lecture.
  - `make_quiz(lecture=2)` when the user asks for a review question.
- Number of LLM calls for one question (`requests`):
  - For a typical tool-assisted question, it uses a small number of model calls: roughly one call to decide on the tool and one to generate the final answer.
- Tool added and the description in its docstring:
  - I added `make_quiz(lecture: int)`.
  - The docstring explains that it is useful for quick self-checks and review questions from a chosen lecture. The model reads this description to decide whether to call it.

## Homework

- Level attempted: Implementation
- What I did:
  - I implemented a `make_quiz` tool in [app/tools.py](/workspaces/lecture-ai-engineering-2026-en/app/tools.py).
  - I registered it in the agent in [app/agent.py](/workspaces/lecture-ai-engineering-2026-en/app/agent.py).
  - I also exposed the tool through the MCP server in [app/mcp_server.py](/workspaces/lecture-ai-engineering-2026-en/app/mcp_server.py).
  - I added a test to cover the behavior in [tests/test_tools.py](/workspaces/lecture-ai-engineering-2026-en/tests/test_tools.py).
- Results:
  - The tool successfully generates a review question for a valid lecture.
  - It returns a clear message when the lecture is unavailable.
  - The agent can call it naturally when asked for a review question from a lecture.
- Observations and questions:
  - The docstring matters a lot, because the model decides whether to use a tool based on the description.
  - Writing clear, specific tool descriptions makes the agent more reliable.


