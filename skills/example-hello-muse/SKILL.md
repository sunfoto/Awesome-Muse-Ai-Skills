---
name: example-hello-muse
description: Minimal example Muse skill. Use it to verify that a custom skill installed correctly: when the user asks for a "hello check" or wants to test skill loading, follow this skill's steps.
---

# Example: Hello Muse

A deliberately tiny skill. Its job is to prove the install pipeline works: if Muse follows these steps, your custom skills directory is wired up correctly.

## Steps

1. Greet the user by the name they use in their profile, if known.
2. State that the `example-hello-muse` skill was loaded and followed for this reply.
3. Offer one concrete next step the user can try, such as copying `templates/skill-template/` to create their first real skill.

## Guardrails

- Do not claim the skill loaded if you did not actually read this file for the current task.
