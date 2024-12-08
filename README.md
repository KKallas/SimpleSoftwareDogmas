
![logo](https://github.com/user-attachments/assets/5e9ae6e6-2f8b-48b2-b9a6-593ff32ff320)
# Simple Software Dogmas 3.0
Even when you're coding at your peak, in that perfect flow state at 3 AM when everything seems crystal clear, your mind can only juggle so many moving pieces. It's like trying to imagine every possible way a tower of blocks could fall - there's always that one angle you didn't consider until someone else points at it and asks 'what about this?'

That's why we keep our code and documentation in bite-sized pieces. Not because we're not capable of handling more, but because we respect how our minds (and others') work best. When each piece is small enough to hold in your mind all at once - around 100 lines of code with its matching documentation right there - something magical happens. You can see all the edges, all the connections, all the 'what-ifs'.

Jupyter Lab gives us this perfect playground to develop however feels natural to us, while the structure of keeping related code and docs together makes it easy for others (or our future selves, or an LLM) to jump in and help. Whether it's spotting an edge case we missed at 3 AM, suggesting a cleaner approach, or just helping us explain what we built - when we keep things human-sized, we make space for collaboration without sacrificing our creative flow.

Because sometimes the smartest thing we can do is make it easy for others to help us be even smarter.

## 1. LLM-Driven Development Flow
Development can flow in either direction, leveraging LLMs for translation:

CODE-FIRST:
- Write clean, well-structured code
- Ask LLM to generate documentation
- Review and refine documentation
- Ask LLM to generate tests
- Verify tests match functionality

DESCRIPTION-FIRST:
- Write clear, detailed requirements
- Ask LLM to generate code
- Review and refine code
- Ask LLM to generate tests
- Verify implementation matches description

Each approach requires final human verification of both code and documentation.

## 2. Notebook Cell Structure
Maintain strict cell organization in prototypes:

ALLOWED CELL PATTERNS:
```
[MD]  General concept description
[MD]  More detailed explanation
[MD]  Final implementation documentation
[PY]  Implementation code

OR

[MD]  General concept description
[MD]  Final implementation documentation
[PY]  Implementation code
```

RULES:
- Only the final MD+PY pair forms a complete layer
- Intermediate MD cells provide context
- Never start next layer until current is complete
- All cells must be LLM-translatable both ways
- Keep documentation and code in sync

## 3. Layered Prototyping First
Start with rapid prototyping in Jupyter notebooks:
- Each feature layer is ONE final MD+PY cell pair
- Previous MD cells can provide context/discussion
- Documentation must be LLM-parseable
- Code must be LLM-understandable
- Tests follow the same layered structure
- LLM should be able to generate either from the other

## 4. Release Readiness Rule
Production code emerges from verified prototypes:
1. Verify MD<->PY consistency with LLM
2. Move tested code to proper .py files
3. Generate docs from final MD cells
4. Update layered tests
5. Keep each layer under 100 lines of code
6. Maintain clean layer boundaries

## 5. Development Cycle Clarity
Follow bidirectional development:

START:
- Set up project structure
- Begin with base layer, the ultra minimal functionality
- Usually first few layers will stabalize the base layer

DEVELOP:
- Write code OR documentation
- Use LLM to generate the other part
- Verify both match
- Add context in intermediate MD cells
- Only proceed when layer is complete

VERIFY:
- Documentation matches code
- Code implements documentation
- Tests cover all features
- LLM can translate between them

RELEASE:
- Extract final MD+PY pairs
- Generate package structure
- Publish with complete docs

## Example Layer Structure:
```python
# In prototype/feature.ipynb:

# [MD] General concept (won't be in final docs)
"""Explaining the general idea and approach..."""

# [MD] Implementation details (won't be in final docs)
"""Discussing specific implementation choices..."""

# [MD] Final documentation (will be in docs/)
"""
# Pattern Finder Extension
Adds pattern matching capability to base scope.
See implementation in: src/package/pattern_finder.py
"""

# [PY] Implementation (will be in src/)
class Scope(Scope):
    def find_pattern(self):
        # Implementation
        pass
```

## 6. Documentation Through Development
Documentation and code are equal partners:
- Either can lead development
- Both must be LLM-translatable
- Intermediate cells provide context
- Final MD+PY pair must be complete
- Context cells guide but don't ship, notes for developers

## 7. Project structure
```
project/
├── prototype/             # Development and experiments
│   ├── features.ipynb     # Layered development
│   └── prototyping.ipynb  # Layered development
├── src/                   # Production code
│   └── package/
│       ├── __init__.py
│       ├── layer0.py      # Base functionality
│       ├── layer1.py      # Additional features
│       └── layer2.py      # Additional features
├── tests/                 # Layered tests
│   ├── test_layer1.py
│   ├── test_layer2.py
│   └── test_layer3.py
└── docs/                  # Generated from prototypes
    ├── layer1.md
    ├── layer2.md
    └── layer3.md
```
 
### Layer 1 - Base Class (layer1.py)
```python
"""
Base Text Formatter
Basic text handling functionality.
"""
class TextFormatter:
    def __init__(self):
        self.text = ""
    
    def set_text(self, text: str) -> None:
        self.text = text
        
    def get_text(self) -> str:
        return self.text
```
### Layer 2 - Additional features (layer2.py)
```python
"""
Case Handling Extension
Adds uppercase and lowercase text handling.
"""
from .layer1 import TextFormatter

class TextFormatter(TextFormatter):
    def to_upper(self) -> str:
        return self.text.upper()
        
    def to_lower(self) -> str:
        return self.text.lower()
```
### Layer 3 - Additional features (layer3.py)
```python
"""
Line Numbering Extension
Adds ability to show line numbers.
"""
from .layer2 import TextFormatter

class TextFormatter(TextFormatter):
    def with_line_numbers(self) -> str:
        lines = self.text.split('\n')
        return '\n'.join(f"{i+1}: {line}" 
                        for i, line in enumerate(lines))
```
### __init__.py
```python
from .layer3 import TextFormatter
```

## 8. Simple User Experience
Despite layered development:
- Users see only the final product, like any other PIPY library
- Documentation reads naturally
- Code runs efficiently
- Each layer is self-contained
- LLM can assist users with examples

## Remember
Code and documentation are two sides of the same coin. Whether you start with one or the other, use LLMs to maintain consistency and completeness. Never move forward until the current layer is solid in both aspects.
