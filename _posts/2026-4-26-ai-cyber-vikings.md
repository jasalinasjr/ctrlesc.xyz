---
layout: post
title: ai cyber vikings
categories: software
---

rl training for valheim.

### tags: updates

moar late-night tinkering turned into a full-on side quest with my valheim ai training bot. been messing around with reinforcement learning (ppo) to get an actual ai agent to play the game instead of just flailing around the meadow like a drunk greyling. the repo is live if you wanna peek: https://github.com/jasalinasjr/valhmeim_ai

goal is pretty straightforward (on paper at least): train it to explore, spot resources, gather wood/stone/mushrooms/berries, survive longer than 30 seconds, and eventually handle combat or basic building. whole setup uses a custom yolo11n model for vision (trained on valheim screenshots), opencv-style screen capture, and direct keyboard/mouse input simulation. config.yaml drives everything so i don’t have to keep editing code for tweaks.

### // v4.1 upgrades

just pushed v4.1 and it’s actually starting to look like progress. switched the health proxy to read the red/blue hud bars more reliably. added novelty bonuses for first-time object detections and progress rewards for moving toward visible resources. 4-frame stacking so the bot has some sense of motion now. potential-based shaping + curiosity rewards to keep it from getting bored and standing still.

training runs in controlled bursts (max steps before auto-save + pause) with gpu temp/vram monitoring so it doesn’t cook my card during long sessions. tensorboard logs, debug screenshots on reward events, separate detection/action logs… the whole monitoring setup is solid.

### // watching it learn

it’s kinda hypnotic scrolling through the debug shots and logs. early versions were pure chaos (v1 was just random wasd + spam attack). now the bot actually spots wood or raspberries and heads straight for them. interacts reliably, health fluctuates realistically, and survival time is climbing. still dies to random shit or gets stuck occasionally, but you can see it learning. greyling detection is there too, though combat needs way more work.

### // next steps

still janky on death/respawn detection (episode resets are hit or miss), stuck penalties, longer stable runs, and better action masking for mouse look. todo.md has the full roadmap – curriculum learning (meadow only at first), episode stats, expanding the yolo model for more biomes, etc. reward hacking opportunities are endless, which is half the fun.

this has been a chill creative outlet to blow off steam and organize my thoughts on rl + game ai. learning a ton while the bot slowly figures out how to not die immediately in the meadows. more updates as it evolves.

### // eof >

that’s it for now.

* an0malous
