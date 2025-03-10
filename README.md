# Structuring Variables in Figma
Variables in Figma store reusable values that can be applied to all kinds of design properties and prototyping actions. They help save time and effort when building designs, managing design systems, and creating complex prototyping flows.

### Differences b/w Variables and Styles
- Variables define reusable values like colors and spacing, while styles are predefined sets of design properties, such as text and effects.
- Variables, unlike styles, enable dynamic design changes across contexts. For instance, switch between light and dark modes or adjust padding for different devices. This allows for adaptable component systems.
- Variables provide design flexibility, allowing instance-specific value changes like button text or color. Styles maintain design consistency for elements like button styles, headings, or color palettes.
- Variables store raw, single values, while styles store sets of values.

### Types of Variables
Variable type | Defined by | &nbsp;
:--- |:--- |:---
`Color` | Solid fills | Color variables are solid values like #000000. Use them for theming (Dark/Light modes) and organizing your palette.
`Number` | Number values | Number variables hold values like 24 or 12.75. Use them for responsive design, language variations, and reusable text styles.
`String` | Text strings | String variables use text (like 'Hello') for language swaps, text styles, and prototype variants.
`Boolean` | True, false values | Boolean variables use true/false. Use them to show/hide layers.

### Collections and Groups
Both collections and groups are used to organize variables and improve their discoverability.

A collection is a set of variables and modes. Collections facilitate the organization of related variables. For example, you might use one collection to localize text across different languages, and another collection for spatial values.

You can further organize variables by placing them into groups within a collection. For instance, use one group for text colors, and another for stroke colors.

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

> Note: 'Design Tokens' and 'Global Primitives' are in sync with the codebase's theme definitions.

## Structuring Within Collections: Groups
#### Design Tokens
Design tokens represent an abstraction in which a referenced value is used.

```
┌───────────────────┐                         
│   Design Tokens   │                         
│    (Published)    │                         
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


#### Global Primitives
```
┌───────────────────┐                                                                  
│ Global Primitives │                                                                  
│    (Published)    │                                                                  
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

#### Language
```
┌───────────────────┐                                  
│     Language      │                                  
│    (Published)    │                                  
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

#### Private Primitives
Private primitives store agnostic values used by global primitives and design token variable collections in Figma. Example: 0, 1, 2, 4, 8, 12... and 100, 200, 300...

```
┌───────────────────┐          
│Private Primitives │          
│    (Examples)     │          
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

