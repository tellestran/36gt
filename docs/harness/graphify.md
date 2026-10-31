# Graphify in this checkout

Graphify is an optional CLI and project-local Agent Skill. The installed skill is
at `.agents/skills/graphify/SKILL.md`; it is not bundled into the gt36 plugin's
`skills/` directory. Open a new Codex chat in this checkout to discover it, or
ask the current agent to read that path explicitly. Repository guidance and the
Codex adapter still apply.

## Set up

Python 3.10+ is required. Run from the repository root:

```sh
python3 -m venv .venv-graphify
.venv-graphify/bin/pip install graphifyy==0.9.99
source .venv-graphify/bin/activate
```

The upstream skill and references are already checked into this integration.
To refresh them intentionally, run `graphify install --project --platform agents`
with the chosen version installed, and review the diff and upstream licenses.
The generic Agent Skill target uses `.agents/skills/`; it adds no agent hooks.

## Build and inspect the code graph

```sh
GRAPHIFY_BACKEND=none graphify extract . --code-only --max-workers 1
GRAPHIFY_BACKEND=none graphify cluster-only . --no-label --backend none
graphify god-nodes --top 5
graphify query "statechart" --budget 1000
graphify export html
```

Read `graphify-out/GRAPH_REPORT.md` and open `graphify-out/graph.html` in a browser.
The runtime and generated files are ignored by Git. `.graphifyignore` excludes
the runtime, output, dependencies, and vendored Graphify skill from extraction.

This code-only graph covers executable helpers. It does **not** explain the
Markdown skills, role prompts, or design rationale that make up most of gt36.
Do not treat missing nodes as evidence that a skill or relationship does not exist.

## Include skills and documentation

In a new Codex chat opened in this checkout, invoke:

```text
/graphify .
```

Or explicitly ask the agent to read `.agents/skills/graphify/SKILL.md` and follow
it using `.venv-graphify/bin/graphify`. The agent can extract concepts from
documents with its own model. Headless document extraction instead needs a
configured model backend; `--code-only` requires no model calls.
Do not run the code-only build over a richer graph unless you intend to replace it.

Source: https://docs.graphify.com/open-source/quickstart
CLI reference: https://docs.graphify.com/open-source/configuration/cli
