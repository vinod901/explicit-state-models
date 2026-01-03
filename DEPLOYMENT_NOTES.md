# Deployment Notes

## Documentation Structure

This repository contains comprehensive documentation exploring implicit vs. explicit state models across industries.

### Documentation Files

```
├── index.md                    # Home page with project overview
├── scenarios/
│   ├── healthcare.md          # Patient discharge coordination
│   └── finance.md             # Commercial loan approval
└── concepts/
    ├── core.md                # States, transitions, invariants
    ├── uncertainty.md         # Confidence and uncertainty
    └── failure-modes.md       # When state models fail
```

## Viewing the Documentation

### Local Markdown Viewing

All documentation is written in standard Markdown and can be viewed:

1. **On GitHub**: Navigate through the repository on github.com
2. **In any Markdown viewer**: VS Code, Typora, or other editors
3. **Using command-line tools**: `less`, `cat`, or `bat` with markdown support
4. **In your IDE**: Most modern IDEs have markdown preview capabilities

All links use relative paths (e.g., `../concepts/core.md`) for compatibility with local viewing.

### No External Hosting Required

This documentation is designed to be read directly from the repository:
- No build process needed
- No GitHub Pages or external hosting
- No Jekyll or static site generator required
- Just plain markdown files

## Key Features

1. **Scenario-Driven**: Two detailed, end-to-end scenarios showing "before" (implicit state) and "after" (explicit state)

2. **Critical Analysis**: Each scenario includes:
   - Detailed problem description
   - Explicit state model solution
   - Benefits analysis
   - **Extensive failure modes and limitations** section
   - **False reporting risks and gaming dynamics**
   - **Practical guardrails to prevent manipulation**
   - Deeper questions raised
   - Acknowledgment that state models are approximations

3. **Visual Aids**: ASCII diagrams illustrating:
   - State transition flows
   - Implicit vs. explicit state visibility
   - Multi-party coordination challenges
   - Gaming dynamics and pressure points

4. **Conceptual Framework**: Core concepts extracted from scenarios, not presented as abstract theory

5. **Cross-Linked**: All pages link together for easy navigation using relative paths

## Recent Updates

### Removed GitHub Pages Hosting
- Deleted `.github/workflows/pages.yml`
- Updated all references to GitHub Pages in README and documentation
- Changed all links to relative paths for local viewing
- Documentation now designed for direct repository viewing

### Added Comprehensive Gaming/False Reporting Analysis
- **Healthcare scenario**: 
  - Detailed section on false reporting when discharge metrics become performance targets
  - 10 specific guardrails to prevent status manipulation by clinical staff
  - Analysis of why staff might game the system (pressure, time constraints, targets)
  
- **Finance scenario**:
  - Section on approval gaming when throughput and approval rates become metrics
  - Lending-specific guardrails (10 strategies)
  - Analysis of unique banking pressures (competitive, relationship, regulatory)
  - Distinction between appropriate business judgment and reckless gaming

### Added Visual Illustrations
- State transition flow diagrams for both healthcare and finance
- Before/after visualization of implicit vs. explicit state
- Multi-party coordination challenge diagrams
- Gaming dynamics pressure visualization

## Content Alignment

✓ Documentation-first, not code-first
✓ Scenario-driven ("before vs after"), not abstract theory
✓ Domain-agnostic patterns through specific domains
✓ Focus on states, transitions, invariants, confidence, uncertainty
✓ Explicitly examines both benefits AND failure modes
✓ **Comprehensive analysis of false reporting risks**
✓ **Practical guardrails for preventing gaming behavior**
✓ **Visual illustrations of key concepts and dynamics**
✓ Precise, neutral, exploratory tone
✓ Avoids hype and solutionism
✓ Treats state models as approximations of reality
✓ Narrative scenarios before abstractions
✓ Shows trade-offs and failure modes

## For Contributors

When adding new content:
- Use relative paths for all internal links
- Keep markdown syntax simple and compatible
- Add ASCII diagrams where they clarify concepts
- Consider how content will read in plain text viewers
- Test links work in both GitHub web UI and local markdown viewers
