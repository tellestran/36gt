# Using Graphify in gt36

The guide is named `GRAPIFY.md` as requested. The tool and command are
**Graphify** / `graphify`; the Python package is `graphifyy`.

Graphify builds a graph of code and document relationships to help you find
related components, understand the project, and trace connections. The local
skill is at [.agents/skills/graphify/SKILL.md](./.agents/skills/graphify/SKILL.md).

## 1. Prepare the environment

Run commands from the gt36 repository root. The `.venv-graphify/` environment
already exists on the current machine. Activate it before using `graphify`:

```sh
source .venv-graphify/bin/activate
graphify --version
```

For a new checkout, install the environment with Python 3.10 or later:

```sh
python3 -m venv .venv-graphify
.venv-graphify/bin/pip install graphifyy==0.9.99
source .venv-graphify/bin/activate
```

Without activation, replace `graphify` in the commands below with
`.venv-graphify/bin/graphify`. Activating a terminal environment does not change
the PATH of another Codex session.

## 2. Use an existing graph

Generated files are in `graphify-out/`:

| File | Purpose |
| --- | --- |
| `graph.html` | Open in a browser to explore nodes and relationships. |
| `GRAPH_REPORT.md` | Read the overview and component groups. |
| `graph.json` | Graph data used by query commands. |

Run these commands in a terminal:

```sh
graphify god-nodes --top 5
graphify query "statechart" --budget 1000
graphify explain "statechart"
graphify path "ComponentA" "ComponentB"
```

Replace component placeholders with names from the graph. Queries match words
in node labels, so start with a specific function, module, or file name. If
several nodes match, use an exact name or path suggested by the tool.

The initial graph contains **code helpers only**. It does not cover the Markdown
skills and role prompts that make up most of gt36. A missing node does not prove
that a component is absent; check the source files.

## 3. Rebuild the code graph without model calls

```sh
GRAPHIFY_BACKEND=none graphify extract . --code-only --max-workers 1
GRAPHIFY_BACKEND=none graphify cluster-only . --no-label --backend none
```

The first command parses code. The second generates the report and HTML.
`.graphifyignore` excludes the runtime, dependencies, and generated output from
extraction. Git ignores the Python environment and `graphify-out/`.

Use this workflow when you want a code-only graph. It can replace a graph that
already includes documents. Avoid this rebuild if you want to retain that richer
document coverage.

## 4. Include skills and documentation through Codex

Open a new Codex chat in this checkout so it can discover the local skill, then enter:

```text
/graphify .
```

If the session does not recognize the command, use an explicit request:

```text
Read .agents/skills/graphify/SKILL.md and docs/harness/codex.md.
Use .venv-graphify/bin/graphify to build a graph of gt36, including
Markdown skills and role prompts, then read and summarize the report.
Follow AGENTS.md and the session's actual permissions.
```

Code parsing runs locally. Semantic document extraction uses the assistant's
model; headless terminal extraction needs a configured model backend. The
upstream skill grants no additional permission to delegate, install hooks, or
change personal configuration. Follow [AGENTS.md](./AGENTS.md) and the
[Codex adapter](./docs/harness/codex.md).

After building the graph, you can ask:

```text
Use the graph and verify against source files to explain relationships
between gt36 skills. Distinguish extracted facts from inferred connections.
```

## 5. Update and verify

To update the code graph after code changes:

```sh
GRAPHIFY_BACKEND=none graphify update .
```

To update document content through the assistant as well:

```text
/graphify . --update
```

Read the report and run a query related to the change. You do not need to rebuild
the graph for each question. If no graph exists, build one first. If an answer
lacks context, check graph coverage and read the source files.

## References

- [Graphify repository](https://github.com/Graphify-Labs/graphify)
- [Quickstart](https://docs.graphify.com/open-source/quickstart)
- [CLI reference](https://docs.graphify.com/open-source/configuration/cli)
- [Installed skill provenance and licenses](./.agents/skills/graphify/UPSTREAM.md)
