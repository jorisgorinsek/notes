### Tips
- Use the high level attack trees in the appendix B of Adam Shostack's book (bookmark this)
- STRIDE is just guidance on which questions you need to ask when identifying threats
	- The answer to a question should trigger more questions. E.g. "how is authentication done?" -> "username and password" -> "how are you onboarded, how is password stored, where is it stored, ..."
- For new technology, ask an AI to explain and list typical security issues with this technology. Possibly also add the way the technology is used / configured
- For threat modeling, the model just needs to be good enough to be able to talk about it and threat model (80% is sufficient)
- Eraser.io to create / enhance diagrams
- When running local models, put the temperature to 0. There's still hallucinations but they become deterministic
- 

#### Speech to text
- transcript of recorded meetings
- Whisper 
	- long silence needs to be removed (e.g. coffee break) to avoid hallucination
	- audio needs to be converted to a specific input as that improves output (WAV, 16-bit mono 16 kHz sample rate)

### Exercise
- If you don't want hallucinations, use an embedded or encoding model iso an LLM to summarize text
- Generate a mermaid diagram first based on a description of the architecture
- Cleanup the diagram,, first using AI, then manually
- ChatGPT gives best result, mermaid code is never correct from first go
- giving examples in the prompt greatly improves the output
- quality of threats and mitigations is significantly lower
- Iterative approach works a lot better. Based on the Socratic way of reasoning.
- The main speedups are: 
	- use AI for first diagram
	- use AI to generate questions to ask during STRIDE, do this iteratively
- StrideGTP with an 8GB lama model seems to work fine for a local model
