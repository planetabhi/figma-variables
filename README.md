# Structuring Figma Variables by Purpose
Building a scalable UI library in Figma starts with organizing variables thoughtfully. Without structure, managing primitives, semantics, and themes becomes chaotic as projects grow. This guide walks through a clear, codebase-aligned approach to structure variables by purpose — from foundational values to reusable design tokens and dynamic content. You'll learn how to reduce redundancy, streamline updates, and ensure consistency across components and abstractions. Let's simplify the complexity.

#### About Variables
Before diving into structuring, it's essential to understand what variables are and why they matter. In essence, variables in Figma store reusable values that can be applied to all kinds of design properties and prototyping actions. They help save time and effort when building designs, managing design systems, and creating complex prototyping flows.

Imagine you're designing a button. Instead of manually setting the color, padding, and border radius every time, you can use variables. If you need to change the button's color across your organization projects, you simply update the variable, and the change propagates everywhere. This is the power of variables.

#### Types of Variables
Figma offers four types of variables, each serving a distinct purpose:

Type | Defined by | &nbsp;
:--- |:--- |:---
`Color` | Solid fills | Solid values like #000000. Used for theming and organizing palette.
`Number` | Number values | Hold values like 16. Used for responsive, language, and text styles.
`String` | Text strings | Use text (like 'Hello') for language, text styles, and prototype variants.
`Boolean` | True, false values | Boolean variables use true/false. Used to show/hide layers.

#### Collections and Groups
To keep your variables organized, Figma provides collections and groups.

- Both collections and groups are used to organize variables and improve their discoverability.
- A collection is a set of variables and modes. Collections facilitate the organization of related variables.
- You can further organize variables by placing them into groups within a collection.

---

## Structuring Collections
Now, let's get to structuring your collections. We'll use four main collections, each with a specific purpose.

```
┌───────────────────┐                                                                  
│    Collections    │                                                                  
└───────────────────┘                                                                  
          │                                                                            
          ├─────────────────────┬─────────────────────┬─────────────────────┐          
          ▼                     ▼                     ▼                     ▼          
┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐
│   Design Tokens   │ │ Global Primitives │ │     Language      │ │Private Primitives │
│    (Published)    │ │    (Published)    │ │    (Published)    │ │      (Hidden)     │
└───────────────────┘ └───────────────────┘ └───────────────────┘ └───────────────────┘                                        
```

&nbsp; | Collection Name | Collection Visibility | Code Definition | &nbsp;
:--- |:--- |:--- |:--- |:---
01 | **Design Tokens** | `Published` | `In sync`  | These provide a meaningful context for how a design primitive should be used. For example, you may have a design token called “color-background-warning” to convey a sense of urgency or potential danger.
02 | **Global Primitives** | `Published` | `In sync` | These are your design system’s basic building blocks, like colors, spacing, and sizing. They form the foundations of your design but aren’t used directly in components or layouts.
03 | **Language** | `Published` | `Optional` | 
04 | **Private Primitives** | `Hidden` | `Not applicable` | 

> Note: 'Design Tokens' and 'Global Primitives' collections can be kept in sync with your codebase's theme definitions, which will be the single source of truth (SSOT).

## Structuring Within Collections: Groups
Now, let’s deep dive into how groups within each collection drive precision and flexibility.

#### 01. Design Tokens Collection
Define colors, spacing, shadows, and sizing for components. Groups enforce consistent styling.

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

In the codebase, `design-tokens.js` might look something like this (rough example):

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

#### 02. Global Primitives Collection
Store base values: colors (grey-90, red-50), font sizes, and weights. Organize them into themes (Default, High Contrast) for quick swaps.

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

#### 03. Language Collection
Holds text strings for gloabl areas, components, patterns, and abstractions. Centralizes translations for design usage.

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

#### 04. Private Primitives Collection
Private primitives contain agnostic values (numbers, scales, black/white) used by global primitives and design token variable collections. These build spacing scales, and colors for other collections, keeping math consistent and reusable.

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
By organizing our Figma variables into these four collections, we create a system that's both robust and scalable. It's like building a city with well-planned streets and neighborhoods, ensuring everything works together seamlessly.

#### In Figma
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
│   │   ├── Content
│   │   └── ...
│   ├── Shadow
│   ├── Sizing
│   ├── Spacing
│   └── ...
├── Global Primitives
│   ├── Colors
│   │   ├── Default theme/
│   │   ├── High contrast/
│   │   └── ...
│   └── Typography
│       ├── Font family
│       ├── Font size
│       ├── Font weight
│       └── Line height
├── Language
│   ├── Global Areas
│   │   ├── Header
│   │   ├── Sidebar
│   │   ├── Footer
│   │   └── ...
│   ├── Patterns
│   └── ...
└── Private Primitives/
```

#### In Code
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

By adopting this structured approach, we not only achieve consistency but also enhance collaboration, scalability, and flexibility. We empower our teams to build better user interfaces, faster.

---

## Misc

#### Differences b/w Variables and Styles
- Variables define reusable values like colors and spacing, while styles are predefined sets of design properties, such as text and effects.
- Variables, unlike styles, enable dynamic design changes across contexts. For instance, switch between light and dark modes or adjust padding for different devices. This allows for adaptable component systems.
- Variables provide design flexibility, allowing instance-specific value changes like button text or color. Styles maintain design consistency for elements like button styles, headings, or color palettes.
- Variables store raw, single values, while styles store sets of values.

## About
Authored by [@planetabhi](https://planetabhi.com/). To provide feedback or suggest improvements, please [open a GitHub issue](https://github.com/planetabhi/figma-variables/issues).