# Structuring Figma Variables by Purpose
Building a scalable UI library in Figma starts with organizing variables thoughtfully. Without structure, managing colors, text, spacing, and themes becomes chaotic as projects grow. This guide walks through a clear, codebase-aligned approach to structure variables by purpose — from foundational values to reusable design tokens and dynamic content. You'll learn how to reduce redundancy, streamline updates, and ensure consistency across components and platforms. Let's simplify the complexity.

#### About Variables
Variables in Figma store reusable values that can be applied to all kinds of design properties and prototyping actions. They help save time and effort when building designs, managing design systems, and creating complex prototyping flows.

#### Types of Variables
Type | Defined by | &nbsp;
:--- |:--- |:---
`Color` | Solid fills | Solid values like #000000. Used for theming and organizing palette.
`Number` | Number values | Hold values like 16. Used for responsive, language, and text styles.
`String` | Text strings | Use text (like 'Hello') for language, text styles, and prototype variants.
`Boolean` | True, false values | Boolean variables use true/false. Used to show/hide layers.

#### Collections and Groups
- Both collections and groups are used to organize variables and improve their discoverability.
- A collection is a set of variables and modes. Collections facilitate the organization of related variables.
- You can further organize variables by placing them into groups within a collection.

---

## Structuring Collections
We will organize our Figma variables using four main collections:

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

&nbsp; | Collection Name | Collection Visibility | Codes Definition
:--- |:--- |:--- |:---
01 | **Design Tokens** | `Published` | `In sync`
02 | **Global Primitives** | `Published` | `In sync`
03 | **Language** | `Published` | `Optional`
04 | **Private Primitives** | `Hidden` | `Not applicable`

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
    'width': { 'default': '' },
    'radius': { 'small': '' }
  },
  'colors': {
    'background': { 'primary': '' },
    'base': { 'primary': '' }
  },
  'shadow': { 'default': '' },
  'sizing': { 'xs': '' },
  'spacing': { 'xs': '' }
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
    'defaultTheme': {
      'grey': { 'grey-90': '', 'grey-00': '' }
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

## Visualizing Variables Structuring
A tree string that visualizes the overall structure of the variables in Figma:

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
│   │   ├── Highlight
│   │   └── more...
│   ├── Shadow
│   ├── Sizing
│   ├── Spacing
│   └── more...
├── Global Primitives
│   ├── Colors
│   │   ├── Default theme/
│   │   ├── High contrast/
│   │   └── more...
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
│   │   └── more...
│   ├── Abstractions
│   ├── Patterns
│   └── more...
└── Private Primitives/
```

---

## Misc

#### Differences b/w Variables and Styles
- Variables define reusable values like colors and spacing, while styles are predefined sets of design properties, such as text and effects.
- Variables, unlike styles, enable dynamic design changes across contexts. For instance, switch between light and dark modes or adjust padding for different devices. This allows for adaptable component systems.
- Variables provide design flexibility, allowing instance-specific value changes like button text or color. Styles maintain design consistency for elements like button styles, headings, or color palettes.
- Variables store raw, single values, while styles store sets of values.

## About
Authored by [@planetabhi](https://planetabhi.com/). To provide feedback or suggest improvements, please [open a GitHub issue](https://github.com/planetabhi/figma-variables/issues).