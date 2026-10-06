### Keeping secrets out of Claude's reach
Your project may contain .env files with API keys or database passwords. You would never commit them to git, so you list them in .gitignore. But .gitignore does not stop Claude from reading them.

That matters for two reasons:

Whatever Claude reads is sent to the model as context
A secret could end up in a summary, a log or a memory file
There is no .claudeignore

You may see people mention a .claudeignore file. It is not an official Claude Code feature. The supported way is a deny rule in your settings.

Add deny rules to the project settings:

.claude/settings.json
``` bash
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./secrets/**)"
    ]
  }
}
The $schema line is optional, but it gives you autocompletion and validation in VS Code.
```

Check the file name

In the project repository this file is committed as .claude/.settings.json, with an extra dot. Claude Code only reads .claude/settings.json (shared) and .claude/settings.local.json (personal). Use the correct name, otherwise your deny rules are silently ignored.


----

What is the context window?
Every time you send a message, Claude does not just see that one message. It sees the whole conversation so far, plus files it has read, command outputs, your CLAUDE.md, loaded skills and Claude Code's own system instructions. All of that together is the context, and every model has a maximum size called the context window.

----
A practical way to combine them:

- Plan the feature with Opus
- Implement it with Sonnet once the plan and instructions are clear
- Use Haiku for small, well-defined follow-up tasks

----

