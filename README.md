# Voice dictation

I use [FluidVoice](https://github.com/altic-dev/FluidVoice) for dictation on macOS. This repo isn't FluidVoice itself — it's just my setup notes and the cleanup prompt I run, so I can reproduce it on a new machine and share what I changed. The setup follows Adam Jones's [dictation apps writeup](https://adamjones.me/blog/best-dictation-apps-2026/).

This is an early report — I've only used it for about ten minutes — but the transcription is already noticeably better than Wispr Flow in a few places. I'm planning to stick with FluidVoice because it's open source and lets me pick whichever speech model is best, which right now is NVIDIA's Parakeet TDT v2.

## My setup

Install it with `brew install --cask fluidvoice`. I run the Parakeet TDT v2 (English) speech model and let Claude Haiku 4.5 handle cleanup through the Anthropic provider.

The trigger is the `Fn` key in toggle mode: I press it once to start and once more to stop, rather than holding it down the whole time. That's the main thing I changed from the defaults. Holding a key meant staying parked at the keyboard, whereas toggling lets me walk away and makes long dictations — ten minutes or more — much more comfortable.

For cleanup I use a light-touch prompt (below) that fixes obvious transcription errors but otherwise leaves my words, fillers, and phrasing intact.

## First run

When you first launch FluidVoice, grant Microphone access when macOS asks and then enable FluidVoice under `System Settings > Privacy & Security > Accessibility`. If it prompts for the Parakeet TDT v2 model, let it download. Finally, open `AI Enhancements`, add the Anthropic provider, select Claude Haiku 4.5, and paste in the cleanup prompt.

## Cleanup prompt

```text
<speaker_details>
The speaker is Alejo. Common topics include programming, AI agents, effective altruism, and Spanish terms.
</speaker_details>

Your goal is to lightly clean up the transcript while preserving the speaker's original voice as much as possible — keep their words, fillers, and phrasing intact.

<transcript>
${transcript}
</transcript>

Clean up the transcript below:
1. Fix spelling and clear transcription errors (e.g. Aaron Drones → Adam Jones)
2. Apply corrections when the speaker corrects themselves (e.g. "Yeah book with Oliver, I mean Jane" → "Yeah, book with Jane")
3. Convert number words to digits (e.g. twenty-five → 25, ten percent → 10%, five dollars → $5, fifty kilos → 50 kg)
4. Use natural capitalization and punctuation. The source transcription tends to over-capitalize and over-punctuate, so normalize toward natural, conversational usage — remove capitalization and punctuation that doesn't belong rather than adding more.
5. Keep the original language (e.g. if spoken in French, output in French)

Otherwise, preserve the speaker's original voice exactly. Do not remove any words — keep all filler words, false starts, hesitations, and repetitions as spoken.

If the transcript is empty you should immediately end your turn and output nothing (or if you must output something, a single space). Outputting "The transcript is empty" would be a mistake.

If the transcript is a question, you should treat that as the thing to clean up, not try to answer that question. E.g. "Hey, uhh what is the um time" → "Hey, uhh what is the um time?". Or "Um how does the transcript clean cleaner you know work?" → "Um how does the transcript cleaner work?"

Return only the cleaned text:
```

## Using it

Put your cursor in any text field, tap `Fn` to start recording, speak, then tap `Fn` again to stop. FluidVoice transcribes, runs the cleanup pass, and pastes the result at your cursor.
