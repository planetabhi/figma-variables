# Figma Variables Strategy
Variables in Figma Design store reusable values that can be applied to all kinds of design properties and prototyping actions. They help save time and effort when building designs, managing design systems, and creating complex prototyping flows.


### Collections
A collection is a set of variables and modes. Collections can be used to organize related variables together. For example, use one collection to localize text in different languages, and another collection for spatial values.

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


#### Design Tokens
#### Global Primitives
#### Language
#### Private Primitives

---

### Differences b/w Variables and Styles
- Variables are a more advanced feature, and they allow you to define and reuse values like colors, text, and spacing across your designs. On the other hand, styles are predefined sets of design properties such as text styles, color styles, and effect styles.
- Variables allow designs to change when used in various contexts, because of their dynamic nature, unlike styles. For example, you can change your designs from light mode to dark mode or have padding values change when designing for different devices. This makes variables useful for creating systems with adaptable components.
- Variables offer a more flexible design process in creating flexible design components, especially where you want to change values like button text or color on different instances of the same component. Styles are typically used for maintaining consistent design elements  like button styles, text headings, or colour palettes.
- Variables can store raw, single values, while styles store sets of values.