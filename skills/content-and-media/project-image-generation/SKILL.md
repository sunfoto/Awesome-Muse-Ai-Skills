---
name: project-image-generation
description: Generate game and project image assets with AI: confirm the art direction with a sample first, build prompts from a proven standard, then clean and deliver each batch.
---

# Generate project images with AI

## Confirm the art direction with a sample first

- Present one or two samples and get approval before generating a full batch. Taste is decided on sight, so a bulk run started before approval gets rejected whole.

- Read the approved sample back as rules the batch must follow, such as motion, surprise effects, or rarity glow tiers. Apply the same rules to every asset in the batch.

## Build prompts from the standard template

- Build each prompt from the nano-banana prompt standard with Scene, Subject, Environment, Lighting, Camera, and Negative prompts, from the `awesome-nano-banana-pro-prompts` collection (YouMind-OpenLab on GitHub — clone it locally and build from its templates). A shared template keeps a batch consistent.

- Keep one base prompt per batch and vary only the subject fields, so style does not drift between assets.

## Generate with the primary tool, fall back on quota

- Generate with Canva AI first. When Canva runs out of quota, switch to Gemini Flow at flow.google.com as the designated fallback instead of waiting.

- Use `media.generate_image` for quick pipeline generations where no style approval is at stake, and pass a reference image the user supplied as the identity reference.

## Clean the downloads and deliver them

- Strip the Gemini icon watermark from Gemini downloads with PIL inpaint using the surrounding area, before handing the files over. Never deliver a watermarked asset.

- When a push tool cannot carry image files, name the exact target paths in the project assets folder and have the user drag and drop the PNGs through the web UI.

- For interactive or animated needs, prefer SVG where the engine supports it. Where it cannot, ship a static base and add the motion as an engine effects overlay instead of fighting the engine.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with the Custom Connectors and MCPs skill, or build a custom connector.

## Alternatives for better results

- Ask the user for a reference image before the batch when one exists, and reuse it across the batch to lock the style.

- Split a large batch across generation sessions, and re-check the first asset of each session against the approved sample before continuing.