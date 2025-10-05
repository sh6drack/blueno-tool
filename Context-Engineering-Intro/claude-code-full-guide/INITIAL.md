## FEATURE:

Build a minimal web-based chat interface for Blueno demonstrating psychodynamic AI therapy approach. Core functionality:
- Clean chat UI with 8-bit/retro aesthetic using monospace fonts
- AI responds using psychodynamic techniques: reflective questions, gentle challenges, pattern exploration
- Conversation persists during session (in-memory only, no backend/accounts)
- Settings panel for basic personalization (theme, font size)
- Simple, calming design with pixel art elements
- Deployable as single HTML file to personal domain


## EXAMPLES:

Reference the provided `bluenoexample.html` file for:
- Overall 8-bit aesthetic approach (pixel art bear, retro styling)
- Color palette: light blue gradients (#89CFF0, #B0E0E6, #87CEEB)
- Monospace font usage: 'Space Mono' for body text, 'Press Start 2P' for headers
- Message bubble styling with shadow effects
- Sidebar layout pattern (can be adapted for settings panel)
- SVG pixel art patterns for avatars and icons

DO NOT COPY the sycophantic placeholder responses in the example file - those demonstrate the WRONG approach.

User: "I've been feeling really anxious about school lately. My parents keep fighting about religion and I just feel stuck."

WRONG (sycophantic validation):
"I'm so sorry you're going through this. That sounds really hard. Your feelings are completely valid. I'm here for you always."

RIGHT (psychodynamic Blueno):
"You mentioned feeling stuck - what does that stuck feeling connect to for you? The anxiety about school, or something about your parents' fights?"

User: "I looked at gay content online and feel terrible about it. What does this mean about me?"

WRONG:
"You're perfect just as you are! There's nothing wrong with exploring your sexuality."

RIGHT:
"What do you think feeling terrible is about? The content itself, or something about having looked at it?"
## DOCUMENTATION:

Psychodynamic therapy techniques for AI responses:
- Ask "what" and "how" questions, avoid "why" (less defensive)
- Reflect back patterns: "You've mentioned feeling stuck twice now..."
- Connect present to past: "How does this remind you of other times you've felt this way?"
- Never provide answers - guide toward self-discovery
- Keep responses 1-2 sentences max
- When user shares difficult content, focus on their feelings about it rather than the content itself
- Avoid: "I'm here for you", "Your feelings are valid", "You're amazing just as you are"
- Use: Questions, reflections, gentle observations about patterns

Claude API system prompt structure should emphasize:
- You are a psychodynamic therapy tool, not a friend or validating companion
- Ask questions that create reflection, not comfort
- Notice and point out patterns, contradictions, repetitions
- Stay curious about what the user is NOT saying
- Brief responses only (1-2 sentences)

## OTHER CONSIDERATIONS:

- 4-hour build timeline - prioritize working demo over perfect code
- Settings panel should include: toggle for response speed, font size adjustment, maybe a "therapy approach intensity" slider
- Include prominent disclaimer: "This is a research prototype demonstrating psychodynamic AI techniques. Not a replacement for professional therapy."
- No localStorage or sessionStorage - keep everything in memory
- Mobile responsive but optimized for laptop demo
- Include 2-3 pre-written conversation starters as buttons users can click to begin
- API key should be easily configurable at top of file
- If Claude API fails, have graceful fallback message
- The bear lamp logo from example file is perfect - keep that branding
- Focus on ONE conversation thread - no history, no multiple chats, just simple linear demo