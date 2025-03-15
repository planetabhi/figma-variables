# Structuring Figma Variables
Building a scalable UI library in Figma begins with thoughtful organization of variables. As projects grow, managing primitives, semantics, and themes without proper structure can quickly become overwhelming. Let's explore a clear, implementation-aligned approach to structuring variables by purpose — from foundational values to reusable design tokens and dynamic content. Together, we'll discover a practical approach to reducing redundancy, streamlining updates, and maintaining consistency across components and abstractions.

#### Understanding Variables in Figma
Before diving into structuring, it helps to understand what variables are and why they matter. At their core, variables in Figma store reusable values that can be applied to various design properties and prototyping actions. They're incredibly useful for saving time and effort when creating designs, managing design systems, and building complex prototyping flows.

Imagine designing a button in your project. Instead of manually setting the color, padding, and border radius each time, you could use variables. When you need to change the button's color across your organization's projects, you simply update the variable once, and the change appears everywhere. That's the magic of variables.

#### Types of Variables
Figma offers four types of variables, each serving a unique purpose:

Type | Defined by | &nbsp;
:--- |:--- |:---
`Color` | Solid fills | Solid values like #000000. Used for theming and organizing palette.
`Number` | Number values | Hold values like 16. Used for responsive, language, and text styles.
`String` | Text strings | Use text (like 'Namaste') for language, text styles, and prototype variants.
`Boolean` | True, false values | Boolean variables use true/false. Used to show/hide layers.

#### Collections and Groups
Figma provides collections and groups to help keep your variables organized:

- Both collections and groups make variables more organized and easier to find.
- A collection brings together related variables and modes, making organization more intuitive.
- Within collections, you can further organize variables by placing them into logical groups.

#### Variables vs. Styles
- Variables define reusable values like colors and spacing, while styles are predefined sets of design properties, such as text formatting and effects.
- Unlike styles, variables enable dynamic design changes across different contexts. For instance, you can easily switch between light and dark modes or adjust padding for different devices, creating more adaptable component systems.
- Variables offer greater design flexibility, allowing you to change specific values like button text or color for individual instances. Styles help maintain design consistency for elements like button styles, headings, or color palettes.
- Variables store individual values, while styles store collections of related values.

---

## Methodology
> Let's organize collections by abstraction levels.

Together, we can organize Figma variables into four collections, each serving a distinct purpose.

```
┌───────────────┐                                                             
│  Collections  │                                                             
└───────────────┘                                                             
        │                                                                     
        ├───────────────────┬──────────────────┬───────────────────┐          
        ▼                   ▼                  ▼                   ▼          
┌───────────────┐ ┌───────────────────┐ ┌─────────────┐ ┌────────────────────┐
│ Design Tokens │ │ Global Primitives │ │  Language   │ │ Private Primitives │
│  (Published)  │ │    (Published)    │ │ (Published) │ │(Hidden & agnostic) │
└───────────────┘ └───────────────────┘ └─────────────┘ └────────────────────┘
```

&nbsp; | Name | Visibility | Code Definition | &nbsp;
:--- |:--- |:--- |:--- |:---
01 | **Design Tokens** | `Published` | `In sync`  | Context for the intended use of a design primitive.
02 | **Global Primitives** | `Published` | `In sync` | Basic building blocks of the design.
03 | **Language** | `Published` | `Optional` | Text strings for adapting to different languages.
04 | **Private Primitives** | `Hidden` | `N/A` | Raw values intended for internal use.

#### The Reasoning Behind This Approach
Through many iterations with enterprise projects, we've developed this collection structure to balance flexibility with governance. It addresses several key challenges we all face when scaling design systems:
- **Clarity in Abstraction**: Separating raw values from semantic tokens aligns design with development. This prevents mixing hex codes with contextual tokens, helping everyone avoid style inconsistencies.
- **Consistency at Scale**: Private primitives help enforce mathematical scales, ensuring uniform spacing throughout interfaces. Designers can use these scaled values rather than manual padding, creating consistent UI element ratios.
- **Efficient Localization**: A dedicated language collection separates text from components, making it simple to switch between languages with minimal effort.
- **Code as the Source of Truth**: "Design Tokens" and "Global Primitives" generate raw, tech-agnostic values. Figma structures mirror codebase themes directly, making design-to-development handoff much smoother.

This approach gives us structure to prevent chaos while preserving the flexibility needed for complex product ecosystems.

---

Let's take a closer look at each collection and explore how the groups within them provide both precision and flexibility.

#### Collection 01: Design Tokens
Design Tokens provide a meaningful context for how a design primitive should be used. For example, a design token called `--background-warning` instantly conveys a sense of urgency or potential caution.

These tokens define colors, spacing, shadows, and sizing for components, with groups that enforce consistent styling across your interface.

- Collection visibility (Figma): `Published`
- Theme definition (Code): `In sync`

```
┌───────────────────┐                         
│   Design Tokens   │                                            
└───────────────────┘                         
          │                                   
          │  ┌────────────┐     ┌────────────┐
          ├─▶│   Border   │──┬─▶│   Width    │
          │  └────────────┘  │  └────────────┘
          │                  │  ┌────────────┐
          │                  └─▶│   Radius   │
          │                     └────────────┘
          │  ┌────────────┐     ┌────────────┐
          ├─▶│   Colors   │──┬─▶│ Background │
          │  └────────────┘  │  └────────────┘
          │  ┌────────────┐  │  ┌────────────┐
          ├─▶│   Shadow   │  ├─▶│    Base    │
          │  └────────────┘  │  └────────────┘
          │  ┌────────────┐  │  ┌────────────┐
          ├─▶│   Sizing   │  ├─▶│   Border   │
          │  └────────────┘  │  └────────────┘
          │  ┌────────────┐  │  ┌────────────┐
          ├─▶│  Spacing   │  ├─▶│  Content   │
          │  └────────────┘  │  └────────────┘
          │  ┌────────────┐  │  ┌────────────┐
          └─▶│   Alias    │  ├─▶│ Highlight  │
             └────────────┘  │  └────────────┘
                             │  ┌────────────┐
                             └─▶│Illustration│
                                └────────────┘
```

In your codebase, `design-tokens.js` might look something like this:

```js
export const designTokens = {
  'border': {
    'width': { 'default': '0.063rem' },
    'radius': { 'small': '0.25rem' }
  },
  'colors': {
    'background': { 'primary': 'oklch(100% 0 0)' },
    'base': { 'primary': 'oklch(59.23% 0.2221 261.8)' }
  },
  'shadow': { 'default': '0px 0px 0px rgba(0, 0, 0, 0);' },
  'sizing': { 'xs': '1rem' },
  'spacing': { 'xs': '0.25rem' }
};
```

#### Collection 02: Global Primitives
Global Primitives form your design system's fundamental building blocks, like colors, spacing, and sizing. They create the foundation of your design but aren't typically used directly in components or layouts.

These primitives store base values such as colors (grey-90, red-50), font sizes, and weights. You can organize them into themes (Default, High Contrast) for quick switching.

- Collection visibility (Figma): `Published`
- Theme definition (Code): `In sync`

```
┌───────────────────┐                                                                  
│ Global Primitives │                                                                 
└───────────────────┘                                                                  
          │                                                                            
          │  ┌────────────┐     ┌────────────────┐     ┌────────────┐    ┌────────────┐
          ├─▶│   Colors   │──┬─▶│ Default theme  │──┬─▶│    Grey    │─┬─▶│  grey-90   │
          │  └────────────┘  │  └────────────────┘  │  └────────────┘ │  └────────────┘
          │                  │  ┌────────────────┐  │  ┌────────────┐ │  ┌────────────┐
          │                  ├─▶│ High contrast  │  ├─▶│    Red     │ ├─▶│  grey-80   │
          │                  │  └────────────────┘  │  └────────────┘ │  └────────────┘
          │                  │  ┌────────────────┐  │  ┌────────────┐ │  ┌────────────┐
          │                  └─▶│ More themes... │  ├─▶│   Orange   │ ├─▶│  grey-70   │
          │                     └────────────────┘  │  └────────────┘ │  └────────────┘
          │                                         │  ┌────────────┐ │  ┌────────────┐
          │                                         ├─▶│   Yellow   │ ├─▶│  grey-60   │
          │                                         │  └────────────┘ │  └────────────┘
          │  ┌────────────┐     ┌────────────────┐  │  ┌────────────┐ │  ┌────────────┐
          └─▶│ Typography │──┬─▶│  Font family   │  ├─▶│   Green    │ ├─▶│  grey-50   │
             └────────────┘  │  └────────────────┘  │  └────────────┘ │  └────────────┘
                             │  ┌────────────────┐  │  ┌────────────┐ │  ┌────────────┐
                             ├─▶│   Font size    │  ├─▶│    Mint    │ ├─▶│  grey-40   │
                             │  └────────────────┘  │  └────────────┘ │  └────────────┘
                             │  ┌────────────────┐  │  ┌────────────┐ │  ┌────────────┐
                             ├─▶│  Font weight   │  ├─▶│    Teal    │ ├─▶│  grey-30   │
                             │  └────────────────┘  │  └────────────┘ │  └────────────┘
                             │  ┌────────────────┐  │  ┌────────────┐ │  ┌────────────┐
                             └─▶│  Line height   │  ├─▶│    Cyan    │ ├─▶│  grey-20   │
                                └────────────────┘  │  └────────────┘ │  └────────────┘
                                                    │  ┌────────────┐ │  ┌────────────┐
                                                    ├─▶│    Blue    │ ├─▶│  grey-10   │
                                                    │  └────────────┘ │  └────────────┘
                                                    │  ┌────────────┐ │  ┌────────────┐
                                                    ├─▶│   Indigo   │ └─▶│  grey-00   │
                                                    │  └────────────┘    └────────────┘
                                                    │  ┌────────────┐                  
                                                    ├─▶│   Purple   │                  
                                                    │  └────────────┘                  
                                                    │  ┌────────────┐                  
                                                    ├─▶│    Pink    │                  
                                                    │  └────────────┘                  
                                                    │  ┌────────────┐                  
                                                    └─▶│   Brown    │                  
                                                       └────────────┘                  
```

Here's a simplified example of what `global.js` could look like in practice:

```js
export const globalPrimitives = {
  'colors': {
    'default': {
      'grey': { 'grey-90': 'oklch(0% 0 0)', 'grey-00': 'oklch(100% 0 0)' }
    }
  },
  'typography': {
    'fontFamily': { 'default': "system-ui, sans-serif;" },
    'fontSize': { 'm': '1rem' },
    'fontWeight': { 'regular': 400 },
    'lineHeight': { 'm': '1.5' }
  }
};
```

#### Collection 03: Language
The Language collection holds text strings for global areas, components, patterns, and abstractions. It centralizes translations, making them accessible throughout your design system.

- Collection visibility (Figma): `Published`
- Theme definition (Code): `Optional`

```
┌───────────────────┐                                  
│     Language      │                                                                    
└───────────────────┘                                  
          │                                            
          │  ┌────────────────┐      ┌────────────────┐
          ├─▶│Global Patterns │───┬─▶│     Header     │
          │  └────────────────┘   │  └────────────────┘
          │  ┌────────────────┐   │  ┌────────────────┐
          ├─▶│  Abstractions  │   ├─▶│    Sidebar     │
          │  └────────────────┘   │  └────────────────┘
          │  ┌────────────────┐   │  ┌────────────────┐
          └─▶│    More...     │   ├─▶│     Footer     │
             └────────────────┘   │  └────────────────┘
                                  │  ┌────────────────┐
                                  └─▶│    More...     │
                                     └────────────────┘
```

#### Collection 04. Private Primitives
Private primitives contain fundamental values (numbers, scales, black/white) that are used by other collections. They help build spacing scales and colors for other collections, ensuring mathematical consistency across your design system.

- Collection visibility (Figma): `Hidden`
- Theme definition (Code): `Not applicable`

```
┌───────────────────┐          
│Private Primitives │                   
└───────────────────┘          
          │                    
          │  ┌────────────────┐
          ├─▶│      zero      │
          │  └────────────────┘
          │  ┌────────────────┐
          ├─▶│   1, 2, 4...   │
          │  └────────────────┘
          │  ┌────────────────┐
          ├─▶│      50vh      │
          │  └────────────────┘
          │  ┌────────────────┐
          ├─▶│  black, white  │
          │  └────────────────┘
          │  ┌────────────────┐
          ├─▶│     solid      │
          │  └────────────────┘
          │  ┌────────────────┐
          └─▶│ regular, bold  │
             └────────────────┘
```

---

## Bringing It All Together
By organizing our Figma variables into these four collections, we create a system that's both robust and scalable. This structured approach enhances collaboration, scalability, and flexibility, empowering our teams to build better user interfaces with greater efficiency.

#### Variables in Figma
```
.
├── Design Tokens
│   ├── Border
│   │   ├── Width
│   │   └── Radius
│   ├── Colors
│   │   ├── Background
│   │   ├── Base
│   │   ├── Border
│   │   └── ...
│   ├── Shadow
│   ├── Sizing
│   ├── Spacing
│   └── ...
├── Global Primitives
│   ├── Colors
│   │   ├── Default theme/
│   │   ├── High contrast theme/
│   │   └── ...
│   └── Typography
│       ├── Font family
│       ├── Font size
│       ├── Font weight
│       └── Line height
├── Language
│   ├── Global areas
│   │   ├── Header
│   │   ├── Sidebar
│   │   ├── Footer
│   │   └── ...
│   ├── Patterns
│   ├── Abstractions
│   └── ...
└── Private Primitives/
```

Our journey doesn't end with organization alone. Let's also make sure our Figma variables stay in sync with the codebase, creating a single source of truth. We can achieve this through custom plugins and automated scripts that bridge the gap between design and development.

#### In the Codebase
```
.
└── themes
    ├── definitions
    │   ├── dark
    │   ├── light
    │   └── tests
    ├── design-tokens.js   // from `Design Tokens` Figma collection
    ├── global.js          // from `Global Primitives` Figma collection
    └── utils.js
```

By embracing this battle-tested approach, we can achieve remarkable consistency while enhancing collaboration across design and development teams. This structure provides the flexibility needed for complex projects while maintaining the rigor required for enterprise-level design systems.

---

## About
Authored by [@planetabhi](https://planetabhi.com/). To provide feedback or suggest improvements, please [open a GitHub issue](https://github.com/planetabhi/figma-variables/issues).