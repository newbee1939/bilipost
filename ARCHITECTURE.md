# Architecture

Only what has been decided is written here. Open questions are at the end.

## Overview

bilipost is a small web app that you put on your own Cloudflare account. You open its URL in a browser and use it. It works the same on a PC and on a phone.

```
Browser ──▶ Server (bilipost) ──▶ LLM (translate)
                              └──▶ X (post)
```

1. You write in the browser
2. The server sends the text to an LLM, gets the translation, and returns it to the browser
3. When you press the post button, the server posts to X

Only the server talks to the LLM and X. The browser only shows the screen and never holds an API key.

## Where it runs

Cloudflare Workers. A service that runs your program for you, so you do not need a server of your own. The program runs only when a request comes in.

- **Why a web app:** It should work on a phone, but without App Store distribution. A web app only needs the phone's browser
- **Why a server in between:** Whatever is sent to the browser can be read by the person using it. An API key sent there would leak, so only the server holds it
- **Why Workers:** Putting the program there gives you a URL that a phone can open. No PC has to stay on, and there is no encryption (TLS) to set up

## Language

TypeScript (JavaScript with types). No external libraries, only what Workers provides.

- **The mainstream on Workers:** The official docs and examples are mostly TypeScript, so answers are easy to find
- **Same language as the screen:** The screen is written in JavaScript anyway. With the server in the same family, there is one language to learn
- **Built-in features are enough:** What is needed (calling external APIs, JSON) is already there, so no external library is added
- **The tool does the conversion:** Workers runs JavaScript, so TypeScript has to be converted. The Cloudflare CLI (`cf`) does this automatically with its Vite plugin, so there is no build step to set up

## Screen

One HTML file and JavaScript without a framework.

- **Served as is:** The HTML goes into Workers static assets (a feature that returns files as they are)
- **No build for the screen:** With React or similar, the code has to be converted into a form the browser can read (a build). One screen does not need that. The HTML and JavaScript run in the browser as written
- **No page reload:** Translating and posting are sent with `fetch` (a browser feature that lets JavaScript talk to the server in the background), and only the result is put into the page. A translation takes a few seconds, and the main use is going back and forth: fix the original, translate again. The input stays in place meanwhile, and "Translating…" can be shown. In return, the waiting state, error messages and double-click protection have to be written by hand

## State

The server stores no data. Once a request is answered, nothing is left.

- **API keys:** Kept in Workers secrets (settings stored encrypted, readable only by the program)
- **Drafts:** Kept in the browser's localStorage (storage inside the browser). They stay only in that browser on that device
- **Post history:** Not kept. It is on X

## Authentication and exposure

bilipost has no login. It leaves that to Cloudflare Access (a Cloudflare feature that asks for a login in front of the app).

- A Workers URL is open to anyone unless you do something. So setting up Access is a required step of the setup
- The server accepts only requests sent from bilipost's own screen

## Why there is no scheduled posting

"Post in three hours" needs a place to store the schedule and something that runs when the time comes. bilipost has neither. Workers can provide both, so adding it starts with revisiting the "State" section.

## Open questions

- **How to call the LLM:** An external LLM API with the user's own key, or Workers AI, which is built into Cloudflare
- **How to authenticate to the X API:** If fixed keys can go into secrets, the current design holds. Tokens that must be refreshed when they expire need a place to be stored, which means revisiting the "State" section
- **How to set it up:** The "Deploy to Cloudflare" button, or deploying from the command line
