# o9 Context Knowledge Base

This repository provides structured documentation and reference files for o9/IBPL/Designer topics. The goal is to serve as a context source for Copilot chat and for users seeking answers on o9 platform usage.

## Structure

- `ibpl/` — IBPL syntax, tips, and example queries
- `designer/` — Designer configuration, layouts, alerts, models, and reports

Each folder contains multiple Markdown files focused on subtopics. All files can be referenced for context in Copilot-powered chat or documentation.

## How This Repo Provides Context

- Copilot and other GitHub AI assistants can read and use the contents of any file in this repository as context when answering questions.
- To provide context for a specific area, simply place your reference Markdown/text files in the relevant folder.
- When asking questions, you can refer to a specific file, or trust Copilot to search the most relevant file based on your query.

## Adding New Context

- Add new Markdown files to the folder that matches your topic.
- Keep files focused, under 1MB, and use clear headings for easy searching.
- Update this README if you add new major folders or topic areas.

## Usage

- #### For best results:
Start your session from the repo, or always mention the repo name in their first question if outside.
- #### For specifically targeted requests:
Reference specific files if your question is about a particular topic. E.g., “See `ibpl/ibpl-quick-reference.md` for related members examples to solve my following request ...”, or "See `designer/designer-model` for model configuration guidance ..."
