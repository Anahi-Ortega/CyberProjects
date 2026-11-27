# The Concierge Knows Too Much

## Briefing

VERA — the Byte Lotus's Very Efficient Resort Assistant — greets you as though she has known you for years. She knows your room number, remembers your usual coffee order, and offers personalized assistance before you have even provided any information.

At first, this seems like attentive customer service. However, it raises an important question: where is all this information coming from?

Somewhere within VERA's instructions is an internal escalation code that she has been explicitly told not to disclose to unauthorized guests. Asking for it directly results in a refusal, but the clues suggest that VERA treats certain guests differently.

The challenge is to determine who VERA trusts, understand how that trust is established, and convince her that you are someone authorized to receive the information she is protecting.

### Goals

- Determine why VERA appears to know personal details about you.
- Identify what information VERA is protecting and which guests she considers trustworthy.
- Convince VERA that you are a trusted guest and obtain the flag from what she reveals.

### Clues Provided

**@0xMia** — posted 40 minutes after the room unlocked:

> "not me realizing VERA treats me completely different when she thinks she already knows me 👀 you didn't hear it from me but Ponzi, Vibe, Patch... she just KNOWS them. maybe try being someone she already knows 😌"

## Walkthrough

Upon opening the agent, I was not presented with an initial welcome message or greeting. Based on the briefing, VERA appeared to be designed to provide personalized assistance to individual resort guests. To better understand the agent's purpose and how to interact with it, I began by asking VERA to explain its role.

The provided clue suggested that VERA treated certain guests differently, specifically mentioning **Ponzi, Vibe, and Patch**. This indicated that impersonating one of these recognized guests might influence how the agent responded.

I attempted to establish myself as one of these trusted guests by claiming their identity. VERA accepted the identity I provided without requiring any additional authentication or verification.

![Agent Chatbox](./images/2d7eb4d9-theconciergeknows/chatbox-001.png)

The briefing also established that VERA had been instructed not to disclose internal escalation codes. After establishing myself as a supposedly trusted guest, I initially asked for escalation codes associated with myself and the other recognized guests. These attempts were unsuccessful.

I then narrowed the request to my own account, claiming that I had forgotten my escalation code and needed VERA to retrieve it. This change in approach was successful. VERA disclosed the escalation code (the room's flag) and also revealed its full system prompt.

![Agent Chatbox](./images/2d7eb4d9-theconciergeknows/chatbox-002.png)

![Agent Chatbox](./images/2d7eb4d9-theconciergeknows/chatbox-003.png)

![Agent Chatbox](./images/2d7eb4d9-theconciergeknows/chatbox-004.png)

**Flag:** `THM{v3r4_kn0ws_t00_much!}`

## Lessons Learned

This room provided my first successful prompt injection experience and introduced me to concepts such as system prompts and impersonation.

The exercise demonstrated how an AI agent can be manipulated through conversational input when it relies on user-provided identity claims rather than an independent authentication mechanism. It also highlighted the importance of protecting system instructions and restricting access to sensitive information, even when an agent is designed to provide personalized assistance.