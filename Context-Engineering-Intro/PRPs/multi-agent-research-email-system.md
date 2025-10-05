name: "Multi-Agent Chat Demo: Research Agent with Email Draft Sub-Agent"
description: |

## Purpose
Build a self-contained Pydantic AI multi-agent system where a primary Research Agent simulates research capabilities and has an Email Draft Agent as a tool. This demonstrates agent-as-tool pattern using only Claude/OpenAI APIs for a complete chat demo.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance

---

## Goal
Create a self-contained multi-agent chat demo where users can ask for research and email drafting via CLI, and the Research Agent can delegate email drafting tasks to an Email Draft Agent. The system should support multiple LLM providers using only LLM APIs.

## Why
- **Demo value**: Showcases multi-agent patterns without external API dependencies
- **Learning**: Demonstrates advanced Pydantic AI multi-agent patterns
- **Simplicity**: Self-contained system using only Claude/OpenAI for complete functionality

## What
A CLI-based chat demo where:
- Users input research queries or email requests
- Research Agent simulates research using LLM knowledge
- Research Agent can invoke Email Draft Agent to create email content
- Results stream back to the user in real-time with tool visibility

### Success Criteria
- [ ] Research Agent simulates research using LLM knowledge
- [ ] Email Agent creates email drafts based on research context
- [ ] Research Agent can invoke Email Agent as a tool
- [ ] CLI provides streaming responses with tool visibility
- [ ] All tests pass and code meets quality standards

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- url: https://ai.pydantic.dev/agents/
  why: Core agent creation patterns
  
- url: https://ai.pydantic.dev/multi-agent-applications/
  why: Multi-agent system patterns, especially agent-as-tool
  
- file: Context-Engineering-Intro/use-cases/pydantic-ai/examples/main_agent_reference/research_agent.py
  why: Pattern for agent creation, tool registration, dependencies
  
- file: Context-Engineering-Intro/use-cases/pydantic-ai/examples/main_agent_reference/providers.py
  why: Multi-provider LLM configuration pattern
  
- file: Context-Engineering-Intro/use-cases/pydantic-ai/examples/main_agent_reference/cli.py
  why: CLI structure with streaming responses and tool visibility

- file: Context-Engineering-Intro/use-cases/pydantic-ai/CLAUDE.md
  why: PydanticAI-specific development rules and patterns
```

### Current Codebase tree
```bash
.
├── Context-Engineering-Intro/
│   ├── examples/
│   │   ├── agent/
│   │   │   ├── agent.py
│   │   │   ├── providers.py
│   │   │   └── ...
│   │   └── cli.py
│   ├── PRPs/
│   │   └── templates/
│   │       └── prp_base.md
│   ├── INITIAL.md
│   ├── CLAUDE.md
│   └── requirements.txt
```

### Desired Codebase tree with files to be added
```bash
.
├── agents/
│   ├── __init__.py               # Package init
│   ├── research_agent.py         # Primary agent with simulated research
│   ├── email_agent.py           # Sub-agent for email drafting
│   ├── providers.py             # LLM provider configuration
│   └── models.py                # Pydantic models for data validation
├── tools/
│   ├── __init__.py              # Package init
│   └── research_tools.py        # Research simulation tools
├── config/
│   ├── __init__.py              # Package init
│   └── settings.py              # Environment and config management
├── tests/
│   ├── __init__.py              # Package init
│   ├── test_research_agent.py   # Research agent tests
│   ├── test_email_agent.py      # Email agent tests
│   ├── test_research_tools.py   # Research tool tests
│   └── test_cli.py              # CLI tests
├── cli.py                       # CLI interface
├── .env.example                 # Environment variables template
├── requirements.txt             # Updated dependencies
└── README.md                    # Comprehensive documentation
```

### Known Gotchas & Library Quirks
```python
# CRITICAL: Pydantic AI requires async throughout - no sync functions in async context
# CRITICAL: Agent-as-tool pattern requires passing ctx.usage for token tracking
# CRITICAL: Always use absolute imports for cleaner code
# CRITICAL: Store LLM API keys in .env, never commit them
# CRITICAL: Use TestModel for unit tests to avoid API calls during testing
# CRITICAL: Multi-agent systems need proper dependency injection patterns
```

## Implementation Blueprint

### Data models and structure

```python
# models.py - Core data structures
from pydantic import BaseModel, Field
from typing import List, Optional
from datetime import datetime

class ResearchQuery(BaseModel):
    query: str = Field(..., description="Research topic to investigate")
    focus_areas: Optional[str] = Field(None, description="Specific areas to focus on")

class ResearchResult(BaseModel):
    topic: str
    summary: str
    key_points: List[str]
    sources: List[str] = Field(default_factory=list)

class EmailDraft(BaseModel):
    to: str = Field(..., description="Recipient email address")
    subject: str = Field(..., min_length=1)
    body: str = Field(..., min_length=1)
    context: Optional[str] = None

class ResearchEmailRequest(BaseModel):
    research_query: str
    email_context: str = Field(..., description="Context for email generation")
    recipient_email: str
```

### List of tasks to be completed

```yaml
Task 1: Setup Configuration and Environment
CREATE config/settings.py:
  - PATTERN: Use pydantic-settings like examples use load_dotenv
  - Load environment variables with defaults
  - Validate required LLM API keys present

CREATE .env.example:
  - Include LLM API keys and configuration
  - Follow pattern from examples/main_agent_reference/.env.example

Task 2: Implement Research Simulation Tools
CREATE tools/research_tools.py:
  - PATTERN: Simple functions that return structured data
  - Simulate research using LLM knowledge and reasoning
  - Return structured ResearchResult models

Task 3: Create Email Draft Agent
CREATE agents/email_agent.py:
  - PATTERN: Follow examples/main_agent_reference/research_agent.py structure
  - Use Agent with deps_type pattern
  - Focus on email composition using LLM capabilities
  - Return structured EmailDraft model

Task 4: Create Research Agent
CREATE agents/research_agent.py:
  - PATTERN: Multi-agent pattern from Pydantic AI docs
  - Register research_tools as tools
  - Register email_agent.run() as tool for multi-agent workflow
  - Use RunContext for dependency injection

Task 5: Implement CLI Interface
CREATE cli.py:
  - PATTERN: Follow examples/main_agent_reference/cli.py streaming pattern
  - Color-coded output with tool visibility
  - Handle async properly with asyncio.run()
  - Session management for conversation context

Task 6: Add Comprehensive Tests
CREATE tests/:
  - PATTERN: Mirror examples test structure
  - Use TestModel to avoid API calls during testing
  - Test happy path, edge cases, errors
  - Ensure 80%+ coverage

Task 7: Create Documentation
CREATE README.md:
  - PATTERN: Follow examples structure
  - Include setup, installation, usage
  - LLM API key configuration steps
  - Multi-agent architecture explanation
```

### Per task pseudocode

```python
# Task 2: Research Simulation Tool
async def simulate_research(query: str, focus_areas: Optional[str] = None) -> ResearchResult:
    """Simulate research using structured reasoning"""
    # PATTERN: Use LLM knowledge to generate realistic research data
    # No external APIs - just structured simulation
    key_points = [
        f"Key finding about {query}",
        f"Important insight regarding {query}",
        f"Critical consideration for {query}"
    ]
    
    # Simulate realistic sources
    sources = [
        f"https://example.com/research/{query.replace(' ', '-')}",
        f"https://academic.example.org/papers/{query.replace(' ', '_')}"
    ]
    
    return ResearchResult(
        topic=query,
        summary=f"Comprehensive research summary about {query}",
        key_points=key_points,
        sources=sources
    )

# Task 4: Research Agent with Email Agent as Tool
@research_agent.tool
async def create_email_draft(
    ctx: RunContext[AgentDependencies],
    recipient: str,
    subject: str,
    context: str
) -> str:
    """Create email draft based on research context."""
    # CRITICAL: Pass usage for token tracking
    result = await email_agent.run(
        f"Create a professional email to {recipient} with subject '{subject}' about: {context}",
        deps=EmailAgentDeps(),
        usage=ctx.usage  # PATTERN from multi-agent docs
    )
    
    return f"Email draft created: {result.data}"
```

### Integration Points
```yaml
ENVIRONMENT:
  - add to: .env
  - vars: |
      # LLM Configuration
      LLM_PROVIDER=openai
      LLM_API_KEY=sk-...
      LLM_MODEL=gpt-4
      
      # Alternative: Claude
      # LLM_PROVIDER=anthropic
      # LLM_API_KEY=sk-ant-...
      # LLM_MODEL=claude-3-sonnet-20240229
      
CONFIG:
  - Self-contained system with no external API dependencies
  - Environment variables only for LLM provider selection
  
DEPENDENCIES:
  - Update requirements.txt with:
    - pydantic-ai
    - python-dotenv
    - rich (for CLI formatting)
    - pytest (for testing)
```

## Validation Loop

### Level 1: Syntax & Style
```bash
# Run these FIRST - fix any errors before proceeding
ruff check . --fix              # Auto-fix style issues
mypy .                          # Type checking

# Expected: No errors. If errors, READ and fix.
```

### Level 2: Unit Tests
```python
# test_research_agent.py
async def test_research_simulation():
    """Test research agent simulates research correctly"""
    from pydantic_ai import TestModel
    # Use TestModel to avoid real API calls
    agent = create_research_agent().override(model=TestModel())
    result = await agent.run("AI safety research")
    assert result.data
    assert "AI safety" in result.data

async def test_research_creates_email():
    """Test research agent can invoke email agent"""
    from pydantic_ai import TestModel
    agent = create_research_agent().override(model=TestModel())
    result = await agent.run(
        "Research AI safety and draft email to john@example.com"
    )
    assert "email" in result.data.lower()

# test_email_agent.py  
async def test_email_agent_creation():
    """Test email agent creates email content"""
    from pydantic_ai import TestModel
    agent = create_email_agent().override(model=TestModel())
    result = await agent.run(
        "Create email to test@example.com about AI research findings"
    )
    assert result.data
    assert isinstance(result.data, str)

async def test_multi_agent_workflow():
    """Test complete multi-agent workflow"""
    from pydantic_ai import TestModel
    research_agent = create_research_agent().override(model=TestModel())
    result = await research_agent.run(
        "Research quantum computing and email the findings to researcher@example.com"
    )
    assert "quantum computing" in result.data.lower()
    assert "email" in result.data.lower()
```

```bash
# Run tests iteratively until passing:
pytest tests/ -v --cov=agents --cov=tools --cov-report=term-missing

# If failing: Debug specific test, fix code, re-run
```

### Level 3: Integration Test
```bash
# Test CLI interaction
python cli.py

# Expected interaction:
# You: Research latest AI safety developments
# 🤖 Assistant: [Streams simulated research results]
# 🛠 Tools Used:
#   1. simulate_research (query='AI safety developments')
#
# You: Create an email draft about this to john@example.com  
# 🤖 Assistant: [Creates email draft content]
# 🛠 Tools Used:
#   1. create_email_draft (recipient='john@example.com', ...)

# Expected: Clean CLI output with tool visibility and proper streaming
```

## Final Validation Checklist
- [ ] All tests pass: `pytest tests/ -v`
- [ ] No linting errors: `ruff check .`
- [ ] No type errors: `mypy .`
- [ ] Research simulation tools work correctly
- [ ] Research Agent invokes Email Agent successfully
- [ ] CLI streams responses with tool visibility
- [ ] Multi-agent workflow completes successfully
- [ ] Error cases handled gracefully
- [ ] README includes clear setup instructions
- [ ] .env.example has all required LLM variables

---

## Anti-Patterns to Avoid
- ❌ Don't hardcode LLM API keys - use environment variables
- ❌ Don't use sync functions in async agent context
- ❌ Don't skip TestModel for unit tests
- ❌ Don't forget to pass ctx.usage in multi-agent calls
- ❌ Don't commit .env files with real API keys
- ❌ Don't add external API dependencies for this demo

## Confidence Score: 9/10

High confidence due to:
- Clear examples to follow from the codebase
- Self-contained design with no external dependencies
- Established patterns for multi-agent systems in Pydantic AI
- Comprehensive validation gates using TestModel
- Well-documented streaming CLI patterns

Minor uncertainty on multi-agent token usage tracking, but patterns are well-established in the documentation.