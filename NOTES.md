I added in CLAUDE.md the information as per instructions:

First of all a line describing the project. Then the 4 sections, Commands (where I kept commands for running the project, tests, lint etc), Conventions (where I kept only conventions, not architectural information),
Architecture (where I kept information about how the project is organized).

I edited the .claude/settings.json file as well to have an allow, ask and deny rule. Without the deny rule my secrets could be leaked, it is important Claude does not read the .env file.

