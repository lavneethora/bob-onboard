# 🎓 bob-onboard

**A custom IBM Bob mode that transforms Bob into a research code onboarding specialist.**

Drop the configuration into any unfamiliar Python research codebase, activate the mode, and Bob produces structured onboarding documentation in minutes — not weeks.

Built solo by [Lavneet Hora](https://github.com/lavneethora) for the [IBM Bob Hackathon](https://lablab.ai/event/ibm-bob-hackathon), May 15–17, 2026.

---

## The Problem

Every researcher who joins a new project hits the same wall. You clone the repo, open the folder, and see hundreds of files across dozens of modules with minimal documentation. You spend days — sometimes weeks — figuring out what's what.

I lived this. When I joined a research group, I spent days trying to understand a complex codebase with multiple abstraction layers and almost no documentation. No onboarding guide existed. No one warned me about placeholder classes masquerading as real implementations.

That experience — wasted time, hidden gotchas, tribal knowledge locked in senior developers' heads — is universal across research labs, open-source ML projects, and academic codebases.

## The Solution

**bob-onboard** is a custom Bob mode (`.bob/custom_modes.yaml`) that turns IBM Bob into a research code onboarding specialist. When activated on any Python research codebase, it systematically:

1. **Discovers** the project structure, entry points, languages, and frameworks
2. **Maps** the architecture with Mermaid class hierarchy diagrams
3. **Surfaces** domain knowledge — paper citations, algorithm references, docstrings — without hallucinating interpretations
4. **Documents** setup procedures, dependencies, and configuration
5. **Hunts for hidden gotchas** — stub classes, `NotImplementedError` sites, TODO/FIXME comments, and unimplemented abstract methods

The output is a single `ONBOARDING.md` file that takes a new contributor from "just cloned this" to "ready to contribute" in one sitting.

## Live Demo: IBM's Adversarial Robustness Toolbox

To prove it works on real-world complexity, I ran the mode on [IBM's Adversarial Robustness Toolbox (ART)](https://github.com/Trusted-AI/adversarial-robustness-toolbox) — a 5.9k-star ML security library with 17 core modules, 90+ notebooks, abstract base class hierarchies across 8 ML frameworks, and four categories of adversarial attacks.

The generated onboarding guide is at [`outputs/art-onboarding/ONBOARDING.md`](outputs/art-onboarding/ONBOARDING.md). Highlights:

- **Architecture diagram** mapping the full `Attack → EvasionAttack/PoisoningAttack/ExtractionAttack/InferenceAttack` hierarchy and the `BaseEstimator → LossGradientsMixin/NeuralNetworkMixin` mixin pattern
- **30+ NotImplementedError sites** identified with exact file paths and line numbers, categorized by type (abstract base classes, framework limitations, optimization gaps)
- **Paper citations** extracted verbatim from source code (FGSM → Goodfellow et al. 2015, PGD → Madry et al. 2018, C&W → Carlini & Wagner 2017)
- **Three complete workflow examples** with working code: robustness evaluation, adversarial training, and backdoor detection
- **TODO/FIXME inventory** grouped by category (performance optimizations, algorithm improvements, feature enhancements)

## How to Install

### Prerequisites

- [IBM Bob IDE](https://www.ibm.com/products/bob) installed and signed in

### Setup (30 seconds)

1. Clone this repo:
   ```bash
   git clone https://github.com/lavneethora/bob-onboard.git
   ```

2. Clone any Python research codebase you want to onboard to:
   ```bash
   git clone https://github.com/SOME-ORG/some-research-repo.git
   ```

3. Copy the mode configuration into the target codebase:
   ```bash
   mkdir -p some-research-repo/.bob
   cp bob-onboard/.bob/custom_modes.yaml some-research-repo/.bob/
   ```

4. Optionally, copy the output template:
   ```bash
   mkdir -p some-research-repo/templates
   cp bob-onboard/templates/onboarding-template.md some-research-repo/templates/
   ```

5. Open the target codebase in Bob:
   - File → Open Folder → select `some-research-repo/`

6. Activate the mode:
   - Click the mode dropdown at the bottom of the chat panel
   - Select **🎓 Research Onboarding**

7. Run the onboarding:
   ```
   I just cloned this codebase and I'm a new researcher trying to onboard.
   Please analyze the repository and produce a comprehensive onboarding
   guide at ONBOARDING.md in the project root.
   ```

Bob will work through five phases (Discovery → Architecture Mapping → Setup Documentation → Documentation Generation → Interactive Support) and produce your `ONBOARDING.md`.

## Repository Structure

```
bob-onboard/
├── .bob/
│   └── custom_modes.yaml          ← The mode definition (the product)
├── templates/
│   └── onboarding-template.md     ← Output scaffold with {{PLACEHOLDER}} syntax
├── outputs/
│   └── art-onboarding/
│       └── ONBOARDING.md          ← Generated guide for ART (proof it works)
├── bob_sessions/                  ← Exported Bob task histories
├── demo/                          ← Video and presentation materials
└── README.md
```

## How It Uses IBM Bob

This project doesn't just use Bob as a coding assistant — **Bob IS the product**. The submission is a Bob configuration package that extends Bob's capabilities through:

- **Custom Mode** (`research-onboard`): A specialized persona with a detailed role definition, systematic five-phase workflow, and file-restricted permissions (markdown-only editing — the mode never touches source code)
- **Template-Driven Output**: A `{{PLACEHOLDER}}`-based scaffold that ensures consistent, comprehensive documentation regardless of which codebase the mode analyzes
- **Tool Group Restrictions**: The mode uses only `read` and `edit` (restricted to `\.md$` files), following least-privilege principles — no command execution, no browser access
- **Native Bob Features**: Custom modes, file restrictions, tool groups, workspace-specific configuration via `.bob/custom_modes.yaml`

Every Bob session used to build this project is exported in `bob_sessions/`.

## The Authentic Story

This isn't a hypothetical problem. When I joined a research group, I spent days mapping a complex codebase with multiple abstraction layers. I discovered an unimplemented placeholder class only after extensive manual exploration. I wrote a detailed tutorial documenting what I learned — the hard way.

**bob-onboard automates what I did manually.** The "Known Incomplete Components" section exists specifically because of that experience. When the mode ran on ART, it found 30+ `NotImplementedError` sites, framework-specific limitations, and TODO markers — exactly the kind of hidden information that wastes new contributors' time.

## Tech Stack

- **IBM Bob IDE** — Custom mode configuration, workspace management
- **YAML** — Mode definition format (`.bob/custom_modes.yaml`)
- **Markdown** — All output artifacts
- **Mermaid** — Architecture diagrams (renders natively on GitHub)

## Built With

- IBM Bob IDE (Enterprise plan, IBM Bob Hackathon instance)
- ~7 Bobcoins used across planning, mode design, template creation, and ART demo run

## License

MIT

## Author

**Lavneet Hora**
- Computer Science (Honors), Texas Tech University — Class of 2027
- [GitHub](https://github.com/lavneethora)
