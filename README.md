# Sarfealam Auto-Studio

Mobile-first studio that turns any idea into a live preview, lets you test it on the phone, then exports a GitHub-ready project for an Android APK.

## Test on a phone browser

1. Install dependencies and start the app (`npm install`, then `npm run dev`).
2. Open the studio from the phone browser on the same network as the computer running it.
3. Co-Pilot greets you. Switch **Hindi / English** at the top. The co-pilot follows that switch.
4. Open **Secrets** once. Keys, tokens, and both Firebase slots stay on the device until you delete them.
5. Open **Build**, type any idea (or tap an example), and press **Launch Build**.
6. Watch the five stages. At stage 4 the on-screen controls unlock. Try them.
7. Press **OK — Continue to Export** only after the test feels right. Download the project zip from the export panel.

Nothing is locked to a famous app name. The pipeline reads the prompt and applies matching skills (account gate, lists, wallet, map-style booking, vehicle controls, card table, and the rest).

## Studio Firebase vs Apps Firebase

They are different slots on purpose.

- **Studio Firebase** stores studio records only: jobs, build status, vault, prompt, and language. On the next visit the studio reloads them, so a refresh does not wipe a running job. You will see “job still running” or “result ready”.
- **Apps Firebase** is the data server for apps you build. When you press OK, that URL and key are written into the generated project (`firebase.apps.json` and `www/app.js`) so profile and user data for the published app go there, including after you push to GitHub.

If Studio Firebase is empty, the same records stay in this browser (`localStorage`) and the vault says they will sync once you connect it. API keys and tokens are never uploaded; they stay in local storage until you delete them.

Use a Realtime Database URL plus its auth secret. You can paste a client config JSON (`databaseURL`, `apiKey`) and apply it.

## How AI departments work

One key can serve many models. Each slot accepts a pasted key and an optional tag: General, Code, Fast, Vision, Primary, or Fallback. The studio also recognizes common prefixes (`AIza`, `gsk_`, `sk-or-`, `nvapi-`, and a few others).

During a build or a co-pilot reply you get a routing line, for example `Code: Qwen 2.5 Coder 32B` or `Code: Groq gpt-oss-120b · fallback`.

Order, top to bottom, skipping providers you did not key:

1. **Brain** — Gemini 2.5 Flash, Groq gpt-oss-120b, Llama 3.3 70B Instruct, Moonshot Kimi K3.1 Pro, Cohere command-a-03-2025, OpenRouter free, Mixtral 8x22B.
2. **Code** — Qwen 2.5 Coder 32B, DeepSeek V4.1 Flash, Gemini 2.5 Flash, Groq gpt-oss-120b, Nemotron Super 49B.
3. **Fast** — DeepSeek V4.1 Flash, Nemotron 3.5 Lightning, Groq, OpenRouter free, Mixtral 8x22B.
4. **Vision** — Nemotron Nano 12B VL, Gemini 2.5 Flash vision. Use **Run UI check** on a build.
5. **Listen** — Groq Whisper large-v3-turbo, NVIDIA Parakeet. The mic uses the phone’s speech recognition and shows which route was selected.
6. **Speak** — NVIDIA Magpie. Playback uses the device voice so it works without a round-trip; the routing line still names Magpie when an NVIDIA key is saved.
7. **Reason** — DeepSeek R1, Llama 3.3 70B, Nemotron Super 49B, Kimi K3.1 Pro.

No keys: the studio stays fully usable on the local assembler. Buttons, stages, vault, language, and secrets still change for real.

## Upload to GitHub for an Android APK

1. In the studio, save GitHub as **Token 1** if you want the on-screen checklist (create repo, push, Actions APK). The checklist is a realistic status path. The zip is the real project.
2. After OK, download the zip. It contains `www/` (open `index.html` to retest the build), `firebase.apps.json`, Capacitor config, and `.github/workflows/android-apk.yml`.
3. Create a repository and push those files.
4. On a machine with Node: `npm install`, then `npx cap add android`, commit the `android` folder, and push `main`.
5. GitHub Actions runs `assembleDebug` and uploads the debug APK artifact.

Admin (banner, mock notification, VIP note, APK link) controls the live preview for the build you have selected. Public users get the exported app; those switches stay with you in the studio.
