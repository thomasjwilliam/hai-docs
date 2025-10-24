# Bring Your Own Theme

You can customize the appearance of the editor and the documents you write by providing your own CSS stylesheet that will be loaded at runtime. This allows you to use your own styling preferences, override default styles, and create custom classes while using the built-in design system.

## How Theming Works

The application uses **CSS custom properties (variables)** as design tokens. These tokens define colors, spacing, and other visual properties throughout the interface. When you create custom CSS, you can reference these same tokens to ensure your styles automatically adapt to theme changes (light/dark mode, custom themes, etc.).

### OKLCH Color Space

All color tokens use the **OKLCH color space** (Oklab Lightness Chroma Hue), a modern perceptually uniform color space that offers significant advantages over traditional RGB/Hex colors:

**Why OKLCH?**
- **Perceptual uniformity**: Changes in OKLCH values correspond to how humans actually perceive color differences
- **Predictable lightness**: The lightness channel directly correlates to perceived brightness, making it easy to create accessible color variations
- **Consistent chroma**: Colors with the same chroma value appear equally vibrant regardless of hue
- **Better gradients**: Interpolating between OKLCH colors produces more natural-looking gradients than RGB
- **Automatic transformations**: Programmatic color manipulation (lightening, darkening, adjusting saturation) produces more predictable and visually pleasing results

**Benefits for theming:**
- Easier to create harmonious color palettes that work in both light and dark modes
- Better accessibility through predictable contrast ratios
- More reliable automatic color generation for hover states, variants, and related colors
- Smoother animations and transitions between theme changes

### Integration with Tailwind

All Tailwind utility classes (like `text-primary`, `bg-surface`, `border-border`) are built on top of these CSS variables. This means:

- Tailwind classes automatically respect theme changes
- Your custom CSS can use the same variables for consistency
- Light and dark modes work seamlessly across all styles
- All colors automatically benefit from OKLCH's perceptual uniformity

## Writing Custom Classes

You can create semantic CSS classes that use design tokens and apply them to directives in your documents.

### Basic Example

**Your custom CSS:**
```css
.card {
  background-color: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  padding: 1.5rem;
}

.card__title {
  color: var(--color-primary);
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.card__content {
  color: var(--color-on-surface);
  line-height: 1.6;
}
```

**In your markdown document:**
```markdown
{{div class="card"}}
  {{div class="card__title"}}Important Notice{{/div}}
  {{div class="card__content"}}
    This is a custom styled card using semantic class names.
  {{/div}}
{{/div}}
```

### BEM (Block Element Modifier) Style

The BEM methodology helps create reusable, maintainable components:

```css
/* Block */
.alert {
  padding: 1rem;
  border-radius: var(--border-radius);
  border-left: 4px solid currentColor;
}

/* Elements */
.alert__icon {
  display: inline-block;
  margin-right: 0.5rem;
}

.alert__message {
  color: var(--color-on-surface);
}

/* Modifiers */
.alert--info {
  background-color: var(--color-info);
  color: var(--color-on-info);
}

.alert--success {
  background-color: var(--color-success);
  color: var(--color-on-success);
}

.alert--warning {
  background-color: var(--color-warning);
  color: var(--color-on-warning);
}

.alert--error {
  background-color: var(--color-error);
  color: var(--color-on-error);
}
```

**Usage:**
```markdown
{{div class="alert alert--success"}}
  {{span class="alert__icon"}}✓{{/span}}
  {{span class="alert__message"}}Operation completed successfully!{{/span}}
{{/div}}
```

### OOCSS (Object-Oriented CSS) Style

OOCSS separates structure from skin for maximum reusability:

```css
/* Structure */
.media {
  display: flex;
  gap: 1rem;
}

.media__figure {
  flex-shrink: 0;
}

.media__body {
  flex-grow: 1;
}

/* Skin */
.box {
  background-color: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  padding: 1rem;
}

.box--elevated {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.text-muted {
  color: var(--color-on-surface-variant);
}
```

**Usage:**
```markdown
{{div class="media box box--elevated"}}
  {{div class="media__figure"}}
    🎨
  {{/div}}
  {{div class="media__body"}}
    {{div}}Design System{{/div}}
    {{div class="text-muted"}}Consistent, scalable styling{{/div}}
  {{/div}}
{{/div}}
```

## Combining Custom CSS with Tailwind

You can mix your custom classes with Tailwind utilities for maximum flexibility:

```css
.feature-card {
  background: linear-gradient(
    135deg, 
    var(--color-primary), 
    var(--color-secondary)
  );
  position: relative;
  overflow: hidden;
}

.feature-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.2);
  opacity: 0;
  transition: opacity 0.3s;
}

.feature-card:hover::before {
  opacity: 1;
}
```

**Usage with Tailwind:**
```markdown
{{div class="feature-card p-6 rounded-lg"}}
  {{div class="text-white text-xl font-bold mb-2"}}
    Premium Feature
  {{/div}}
  {{div class="text-white opacity-90"}}
    Enhanced capabilities for power users
  {{/div}}
{{/div}}
```

## Best Practices

### 1. Use Design Tokens
Always reference CSS variables rather than hardcoding values:

✅ **Good:**
```css
.highlight {
  background-color: var(--color-primary);
  color: var(--color-on-primary);
}
```

❌ **Avoid:**
```css
.highlight {
  background-color: #3b82f6;
  color: white;
}
```

### 2. Semantic Class Names
Choose names that describe purpose, not appearance:

✅ **Good:** `.notification`, `.error-message`, `.primary-action`

❌ **Avoid:** `.red-box`, `.big-text`, `.blue-button`

### 3. Scope Your Styles
Prefix your custom classes to avoid conflicts:

```css
.myapp-header { }
.myapp-navigation { }
.myapp-footer { }
```

### 4. Leverage Tailwind for Common Patterns
Use your custom CSS for unique designs and Tailwind for standard utilities:

```markdown
{{div class="custom-gradient p-4 rounded-lg shadow-md"}}
  Mixed approach: custom gradient + Tailwind utilities
{{/div}}
```

---

## ⚠️ Important Notice

**This is under active development.** The CSS variable naming, available tokens, and theming system may change in future updates.

**Feedback welcome!** If you have suggestions for:
- Additional CSS variables that would be useful
- Improvements to the theming system
- Documentation clarifications
- Support for your Use cases

---

## Tokens

All available CSS custom properties (tokens) you can use in your custom stylesheets. These variables automatically adapt to the current theme (light/dark mode).

### Layout

| Token             | Description                           | Why It's Used                                                      |
|-------------------|---------------------------------------|--------------------------------------------------------------------|
| `--border-radius` | Default border radius for UI elements | Provides consistent rounded corners throughout the interface (4px) |

### Background & Surfaces

| Token                        | Description                                  | Why It's Used                                                                                                    |
|------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `--color-bg`                 | Primary page or app background               | Sets the base background for the interface; all other layers stack on top                                        |
| `--color-on-bg`              | Default text color on the background         | Ensures legibility against the main page/app background                                                          |
| `--color-surface`            | Surface background for cards, panels, modals | Provides a separate "layer" from the background for components, giving hierarchy and depth                       |
| `--color-on-surface`         | Default text color on surfaces               | Ensures legible content on top of surface elements                                                               |
| `--color-surface-variant`    | Slightly elevated or alternate surface       | Used for components like menus, popovers, or elevated cards to create contrast without changing the main surface |
| `--color-on-surface-variant` | Default text color on alternative surfaces   | Ensures legible content on variant surface elements                                                              |

### Text

| Token          | Description                | Why It's Used                                        |
|----------------|----------------------------|------------------------------------------------------|
| `--color-text` | General purpose text color | Default text color for body copy and general content |

### Borders & Dividers

| Token            | Description           | Why It's Used                                                                         |
|------------------|-----------------------|---------------------------------------------------------------------------------------|
| `--color-border` | Standard border color | Defines boundaries between elements, used for inputs, cards, and structural divisions |

### Identity Colors

| Token                  | Description                       | Why It's Used                                                            |
|------------------------|-----------------------------------|--------------------------------------------------------------------------|
| `--color-primary`      | Main color                        | Used for primary actions, buttons, links; central to the visual identity |
| `--color-on-primary`   | Text or icons on primary color    | Ensures legibility when content is placed on primary-colored elements    |
| `--color-secondary`    | Secondary color                   | Provides a complementary color for secondary actions or accents          |
| `--color-on-secondary` | Text or icons on secondary color  | Ensures contrast when content is placed on secondary elements            |
| `--color-accent`       | Optional tertiary or accent color | Adds visual interest or highlights without overriding primary/secondary  |
| `--color-on-accent`    | Text/icons on accent surfaces     | Ensures legibility on accent areas                                       |

### State Colors

| Token                | Description                            | Why It's Used                                                 |
|----------------------|----------------------------------------|---------------------------------------------------------------|
| `--color-info`       | Informational state                    | Provides a neutral or informative accent for messages or tips |
| `--color-on-info`    | Text/icons on informational surfaces   | Ensures legibility on info-colored elements                   |
| `--color-success`    | Positive state (confirmation, success) | Used for success messages, badges, and notifications          |
| `--color-on-success` | Text/icons on success surfaces         | Ensures contrast and readability                              |
| `--color-warning`    | Warning state                          | Indicates caution or important alerts                         |
| `--color-on-warning` | Text/icons on warning surfaces         | Ensures legibility on warning-colored elements                |
| `--color-error`      | Error state                            | Highlights errors or destructive actions                      |
| `--color-on-error`   | Text/icons on error surfaces           | Ensures legibility for error messages or icons                |

### Usage Examples

**Basic usage:**
```css
.card {
  background-color: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  padding: 1.5rem;
}

.card__title {
  color: var(--color-primary);
}

.card__content {
  color: var(--color-on-surface);
}
```

**State-based styling:**
```css
.notification {
  padding: 1rem;
  border-radius: var(--border-radius);
}

.notification--info {
  background-color: var(--color-info);
  color: var(--color-on-info);
}

.notification--success {
  background-color: var(--color-success);
  color: var(--color-on-success);
}

.notification--warning {
  background-color: var(--color-warning);
  color: var(--color-on-warning);
}

.notification--error {
  background-color: var(--color-error);
  color: var(--color-on-error);
}
```

**Color buttons:**
```css
.btn {
  padding: 0.5rem 1rem;
  border-radius: var(--border-radius);
  border: none;
  cursor: pointer;
}

.btn--primary {
  background-color: var(--color-primary);
  color: var(--color-on-primary);
}

.btn--secondary {
  background-color: var(--color-secondary);
  color: var(--color-on-secondary);
}
```
