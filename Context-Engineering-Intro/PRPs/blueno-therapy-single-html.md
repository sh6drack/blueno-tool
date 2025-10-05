name: "Blueno Psychodynamic Therapy Single-File HTML App"
description: |

## Purpose
Build a single-file HTML application demonstrating psychodynamic AI therapy techniques using Claude API. A self-contained web chat interface with 8-bit aesthetic that can be deployed to any static hosting.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance

---

## Goal
Create a working single-file HTML therapy chat interface that demonstrates psychodynamic AI techniques, deployable as static file to personal domain with 8-bit/retro aesthetic.

## Why
- **Demo value**: Showcases psychodynamic AI therapy approach vs. sycophantic validation
- **Simplicity**: Single HTML file eliminates deployment complexity
- **Educational**: Clear contrast between therapeutic techniques and standard chatbot responses

## What
A single HTML file containing:
- Clean 8-bit themed chat interface 
- Claude API integration via JavaScript fetch()
- Psychodynamic therapy response patterns
- Settings panel for personalization
- In-memory conversation persistence
- Mobile-responsive design

### Success Criteria
- [ ] Single HTML file runs in browser without external dependencies
- [ ] Claude API integration works with proper error handling
- [ ] AI responds using psychodynamic techniques (not validation)
- [ ] 8-bit aesthetic matches example file styling
- [ ] Settings panel allows basic customization
- [ ] Mobile responsive layout
- [ ] Prominent therapy disclaimer included

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: Context-Engineering-Intro/claude-code-full-guide/INITIAL.md
  why: Detailed requirements and psychodynamic technique examples
  
- file: Context-Engineering-Intro/examples/bluenoexample.html
  why: Complete styling patterns, layout, and 8-bit aesthetic
  
- url: https://docs.anthropic.com/en/api/messages
  why: Claude API messages endpoint documentation
  
- url: https://collabnix.com/claude-api-integration-guide-2025-complete-developer-tutorial-with-code-examples/
  why: JavaScript fetch patterns for Claude API integration
```

### Current Codebase tree
```bash
Context-Engineering-Intro/
├── examples/
│   └── bluenoexample.html           # Styling and layout reference
├── claude-code-full-guide/
│   └── INITIAL.md                   # Feature requirements
└── PRPs/
    └── templates/
        └── prp_base.md             # PRP template
```

### Desired output
```bash
.
└── blueno-therapy.html              # Single deployable HTML file
```

### Known Gotchas & Library Quirks
```javascript
// CRITICAL: Never expose Claude API key in frontend code - use proxy or environment
// CRITICAL: Claude API requires CORS handling for browser requests
// CRITICAL: API rate limits - implement proper error handling
// CRITICAL: Mobile viewport scaling for pixel art elements
// CRITICAL: CSS animations performance on mobile devices
// GOTCHA: SVG pixel art requires image-rendering: pixelated for crisp edges
// GOTCHA: Google Fonts may block on some networks - include fallbacks
```

## Implementation Blueprint

### Core HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>blueno therapy</title>
    <!-- PATTERN: Follow exact font imports from bluenoexample.html -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Space+Mono:wght@400;700&display=swap');
        /* All CSS embedded here */
    </style>
</head>
<body>
    <!-- PATTERN: Use exact layout structure from bluenoexample.html -->
    <div class="app-container">
        <aside class="sidebar"><!-- Settings panel instead of session info --></aside>
        <main class="main-content">
            <div class="chat-header"><!-- Include disclaimer --></div>
            <div class="chat-messages" id="messages"></div>
            <div class="chat-input-container"><!-- Conversation starters --></div>
        </main>
    </div>
    <script>
        // All JavaScript embedded here
    </script>
</body>
</html>
```

### List of tasks to be completed

```yaml
Task 1: Setup Core HTML Structure and Styling
CREATE blueno-therapy.html base structure:
  - PATTERN: Copy exact layout from bluenoexample.html
  - MODIFY sidebar for settings panel instead of session info
  - PRESERVE all styling: fonts, colors, animations, pixel grid
  - ADD therapy disclaimer in chat header

Task 2: Implement Settings Panel
MODIFY sidebar content:
  - PATTERN: Use session-card styling for settings sections
  - ADD font size slider (12px-18px range)
  - ADD response speed toggle (fast/slow)
  - ADD therapy intensity slider (gentle/direct)
  - ADD API key configuration input (with security warning)

Task 3: Add Conversation Starters
MODIFY chat-input-container:
  - PATTERN: Use pixel-button styling
  - ADD 3 conversation starter buttons above input
  - EXAMPLES: "I'm feeling anxious...", "I keep thinking about...", "I'm struggling with..."
  - REMOVE on first user input to keep interface clean

Task 4: Implement Claude API Integration
ADD JavaScript API integration:
  - PATTERN: Use fetch() with proper error handling
  - HANDLE CORS issues with appropriate headers
  - IMPLEMENT rate limiting and retry logic
  - ADD graceful fallback messages for API failures

Task 5: Implement Psychodynamic Response System
CREATE AI interaction logic:
  - PATTERN: System prompt emphasizing psychodynamic techniques
  - AVOID sycophantic validation responses
  - FOCUS on reflective questions and pattern observation
  - IMPLEMENT 1-2 sentence response limits

Task 6: Add Message Display and Memory
IMPLEMENT chat functionality:
  - PATTERN: Use exact message styling from bluenoexample.html
  - ADD typing indicator animation during API calls
  - STORE conversation in memory (no persistence)
  - HANDLE message overflow with scroll

Task 7: Mobile Optimization and Polish
FINALIZE responsive design:
  - PATTERN: Use existing mobile breakpoints from example
  - TEST pixel art scaling on mobile
  - VERIFY touch targets are adequate
  - ADD loading states and error messages
```

### Per task pseudocode

```javascript
// Task 4: Claude API Integration
const API_CONFIG = {
    apiKey: '', // User configurable
    endpoint: 'https://api.anthropic.com/v1/messages',
    model: 'claude-3-sonnet-20240229'
};

async function callClaudeAPI(userMessage, conversationHistory) {
    // PATTERN: Build system prompt for psychodynamic approach
    const systemPrompt = `You are a psychodynamic therapy tool, not a friend or validating companion.
    
Key principles:
- Ask "what" and "how" questions, avoid "why" (less defensive)
- Reflect back patterns: "You've mentioned feeling stuck twice now..."
- Connect present to past experiences
- Never provide answers - guide toward self-discovery
- Keep responses 1-2 sentences maximum
- Focus on feelings about content, not the content itself
- Avoid: "I'm here for you", "Your feelings are valid"
- Use: Questions, reflections, gentle observations about patterns`;

    const messages = [
        { role: 'system', content: systemPrompt },
        ...conversationHistory,
        { role: 'user', content: userMessage }
    ];

    try {
        const response = await fetch(API_CONFIG.endpoint, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'anthropic-version': '2023-06-01',
                'x-api-key': API_CONFIG.apiKey
            },
            body: JSON.stringify({
                model: API_CONFIG.model,
                max_tokens: 150, // Force brevity
                messages: messages
            })
        });

        if (!response.ok) {
            throw new Error(`API Error: ${response.status}`);
        }

        const data = await response.json();
        return data.content[0].text;
    } catch (error) {
        console.error('Claude API Error:', error);
        return "I'm having trouble connecting right now. What would you like to explore while we wait?";
    }
}

// Task 5: Psychodynamic Message Processing
function processUserInput(input) {
    // PATTERN: Look for emotional patterns and deflection
    const emotionalWords = ['anxious', 'stuck', 'terrible', 'confused'];
    const hasEmotion = emotionalWords.some(word => input.toLowerCase().includes(word));
    
    // Add context flags for AI to pick up on
    const context = {
        hasEmotionalContent: hasEmotion,
        isFirstMessage: conversationHistory.length === 0,
        previousPatterns: extractPatterns(conversationHistory)
    };
    
    return context;
}
```

### Integration Points
```yaml
STYLING:
  - Exact CSS from bluenoexample.html with modifications
  - Color palette: #89CFF0, #B0E0E6, #87CEEB, #5dade2, #3498db
  - Fonts: 'Press Start 2P' for headers, 'Space Mono' for body
  - Pixel art bear lamp logo (modified for therapy context)

API:
  - Claude Messages API endpoint
  - System prompt optimized for psychodynamic responses
  - Error handling for network issues and rate limits
  - CORS considerations for browser deployment

SECURITY:
  - API key configuration with security warnings
  - No sensitive data persistence
  - Proper error message handling (no API details exposed)
```

## Validation Loop

### Level 1: File Structure & Basic Functionality
```bash
# Open in browser and test basic functionality
open blueno-therapy.html

# Expected: Page loads with 8-bit styling
# Expected: Chat interface renders correctly
# Expected: Settings panel shows configuration options
```

### Level 2: API Integration Testing
```javascript
// Test API integration with sample conversation
// In browser console:
console.log('Testing API integration...');

// Expected: API calls complete without CORS errors
// Expected: Psychodynamic responses (not validating responses)
// Expected: Graceful error handling when API fails

// Test conversation flow:
// User: "I feel terrible about something I did"
// Expected AI: "What does feeling terrible connect to for you?"
// NOT: "That sounds really hard, your feelings are valid"
```

### Level 3: End-to-End User Experience
```bash
# Test complete user workflow:
# 1. Open file in browser
# 2. Configure API key in settings
# 3. Click conversation starter
# 4. Have multi-turn conversation
# 5. Test on mobile device
# 6. Test error scenarios (invalid API key, network issues)

# Expected behaviors:
# - Clean 8-bit aesthetic maintains on all screen sizes
# - Conversation starters disappear after first interaction
# - AI consistently uses psychodynamic techniques
# - Error messages are user-friendly
# - Settings persist during session
```

## Final Validation Checklist
- [ ] Single HTML file opens and runs in browser
- [ ] 8-bit aesthetic matches bluenoexample.html
- [ ] Claude API integration works with proper error handling
- [ ] AI uses psychodynamic techniques (no sycophantic validation)
- [ ] Settings panel allows font size, response speed, intensity adjustment
- [ ] Conversation starters work and disappear appropriately
- [ ] Mobile responsive layout functions correctly
- [ ] Therapy disclaimer prominently displayed
- [ ] API key configuration includes security warnings
- [ ] Graceful error handling for API failures
- [ ] No external dependencies (except fonts and API)

---

## Anti-Patterns to Avoid
- ❌ Don't create multiple files - everything in one HTML file
- ❌ Don't expose API keys in code - require user configuration
- ❌ Don't use sycophantic AI responses - focus on psychodynamic techniques
- ❌ Don't ignore mobile optimization - pixel art must scale properly
- ❌ Don't skip error handling - API calls will fail
- ❌ Don't add complex features - stick to 4-hour build timeline
- ❌ Don't use localStorage - keep everything in memory

## Confidence Score: 8/10

High confidence due to:
- Clear styling patterns from bluenoexample.html
- Well-defined psychodynamic response requirements
- Established Claude API integration patterns
- Simple single-file deployment model

Minor uncertainty on:
- CORS handling for Claude API from browser
- Optimal system prompt for consistent psychodynamic responses
- Mobile performance with CSS animations and pixel art