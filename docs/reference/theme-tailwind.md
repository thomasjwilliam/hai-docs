# Theming with Tailwind CSS

## About Tailwind CSS

**Tailwind CSS** is a utility-first CSS framework that allows you to build custom designs directly in your markup by applying pre-defined CSS classes. Instead of writing custom CSS, you compose designs using utility classes like `flex`, `pt-4`, `text-center`, and more.

**Version:** This project uses **Tailwind CSS v3**.

**Key Benefits:**
- **Utility-first approach** - Build complex designs without leaving your HTML
- **Responsive design** - Built-in responsive modifiers for every breakpoint
- **Dark mode support** - Easy theme switching with the `dark:` prefix
- **Theme-aware colors** - All color classes use CSS variables that adapt to your theme

---

## Using Tailwind with Directives

You can style directives by adding Tailwind classes to the `class` attribute. This works with any directive that supports the `class` attribute (`note`, `div`, `span`, `input`, `button`, etc.).

### Basic Examples

**Styled note with padding and background:**
```
{{note class="p-6 bg-primary-50 rounded-lg"}}
  This note has padding, a light primary background, and rounded corners.
{{/note}}
```

**Flex layout with gap:**
```
{{div class="flex gap-4 items-center"}}
  {{input placeholder="First Name"}}
  {{input placeholder="Last Name"}}
{{/div}}
```

**Grid layout:**
```
{{div class="grid grid-cols-2 gap-4"}}
  {{div class="p-4 bg-surface border rounded"}}Item 1{{/div}}
  {{div class="p-4 bg-surface border rounded"}}Item 2{{/div}}
  {{div class="p-4 bg-surface border rounded"}}Item 3{{/div}}
  {{div class="p-4 bg-surface border rounded"}}Item 4{{/div}}
{{/div}}
```

**Centered content:**
```
{{div class="flex items-center justify-center h-64"}}
  {{div class="text-center"}}
    # Centered Content
    This is perfectly centered!
  {{/div}}
{{/div}}
```

**Text styling:**
```
{{span class="text-xl font-bold text-primary"}}Important Text{{/span}}
{{span class="text-sm text-text-500 italic"}}Secondary note{{/span}}
```

### Responsive Design with Container Queries

Tailwind in this project uses **container queries** instead of viewport media queries. This means responsive classes work based on the container size, not the screen size.

**Container query breakpoints:**
- `@sm:` - 24rem (384px)
- `@md:` - 28rem (448px)
- `@lg:` - 32rem (512px)
- `@xl:` - 36rem (576px)
- `@2xl:` - 42rem (672px)
- `@3xl:` - 48rem (768px)
- `@4xl:` - 56rem (896px)
- `@5xl:` - 64rem (1024px)
- `@6xl:` - 72rem (1152px)
- `@7xl:` - 80rem (1280px)

**Important:** You must add the `@container` class to the parent element for container queries to work.

**Example - Responsive grid:**
```
{{div class="@container"}}
  {{div class="grid grid-cols-1 @md:grid-cols-2 @lg:grid-cols-3 gap-4"}}
    {{div class="p-4 bg-surface"}}Card 1{{/div}}
    {{div class="p-4 bg-surface"}}Card 2{{/div}}
    {{div class="p-4 bg-surface"}}Card 3{{/div}}
  {{/div}}
{{/div}}
```

**Example - Responsive text alignment:**
```
{{div class="@container"}}
  {{div class="text-center @md:text-left @lg:text-right"}}
    This text is centered on small screens, left-aligned on medium, and right-aligned on large screens.
  {{/div}}
{{/div}}
```

**Example - Show/hide based on container size:**
```
{{div class="@container"}}
  {{span class="@md:hidden"}}Mobile view{{/span}}
  {{span class="hidden @md:block"}}Desktop view{{/span}}
{{/div}}
```

### Dark Mode

Use the `dark:` prefix to apply styles only in dark mode:

```
{{div class="bg-white dark:bg-gray-900 text-black dark:text-white p-4"}}
  This content adapts to light and dark themes
{{/div}}
```

**Dark mode is currently available for:**
- Opacity utilities (`opacity-*` and `dark:opacity-*`)
- Shadow utilities (`shadow-*` and `dark:shadow-*`)

---

## Important Notes

⚠️ **Curated Class List**

This project does **not** include all Tailwind CSS classes. Only a carefully selected subset is available to keep the CSS file size manageable. The current class list includes the most commonly used utilities for:

- Layout (flex, grid, display)
- Spacing (margin, padding)
- Sizing (width, height)
- Typography (text size, weight, alignment)
- Colors (all theme colors with shades 50-900)
- Borders and border radius
- Shadows and opacity
- Common transforms and transitions

**The class list is under review and may change in future updates.** Classes may be added or removed based on usage patterns and performance considerations.

🎨 **Theme-Aware Colors**

All color classes (text, background, border) use CSS variables that automatically adapt when users switch themes. This means:
- `text-primary` will use the current theme's primary color
- `bg-success-100` will use the current theme's success-100 shade
- Colors update in real-time when themes change

**Available theme colors:** `primary`, `secondary`, `accent`, `info`, `success`, `warning`, `error`, `surface`, `text`, `bg`

Each color has shades: `50`, `100`, `200`, `300`, `400`, `500`, `600`, `700`, `800`, `900`

---

## Available Classes Reference

### Display & Visibility

**Display:**
`block`, `inline-block`, `inline`, `flex`, `inline-flex`, `grid`, `inline-grid`, `table`, `table-caption`, `table-cell`, `table-column`, `table-column-group`, `table-footer-group`, `table-header-group`, `table-row-group`, `table-row`, `flow-root`, `contents`, `list-item`, `hidden`

**Visibility:**
`visible`, `invisible`

### Position & Z-Index

**Position:**
`static`, `fixed`, `absolute`, `relative`, `sticky`

**Inset:**
`inset-0`, `inset-x-0`, `inset-y-0`, `top-0`, `top-1`, `top-2`, `top-3`, `top-4`, `top-auto`, `right-0`, `right-1`, `right-2`, `right-3`, `right-4`, `right-auto`, `bottom-0`, `bottom-1`, `bottom-2`, `bottom-3`, `bottom-4`, `bottom-auto`, `left-0`, `left-1`, `left-2`, `left-3`, `left-4`, `left-auto`

**Z-Index:**
`z-0`, `z-10`, `z-20`, `z-30`, `z-40`, `z-50`, `z-auto`

### Container Queries

**Container:**
`@container` - Mark an element as a container for container queries

### Grid System

**Grid Template Columns:**
`grid-cols-1`, `grid-cols-2`, `grid-cols-3`, `grid-cols-4`, `grid-cols-5`, `grid-cols-6`, `grid-cols-7`, `grid-cols-8`, `grid-cols-9`, `grid-cols-10`, `grid-cols-11`, `grid-cols-12`, `grid-cols-none`

**Grid Template Rows:**
`grid-rows-1`, `grid-rows-2`, `grid-rows-3`, `grid-rows-4`, `grid-rows-5`, `grid-rows-6`, `grid-rows-none`

**Grid Auto Flow:**
`grid-flow-row`, `grid-flow-col`, `grid-flow-dense`, `grid-flow-row-dense`, `grid-flow-col-dense`

**Grid Auto Columns:**
`auto-cols-auto`, `auto-cols-min`, `auto-cols-max`, `auto-cols-fr`

**Grid Auto Rows:**
`auto-rows-auto`, `auto-rows-min`, `auto-rows-max`, `auto-rows-fr`

### Grid Positioning

**Column Span:**
`col-auto`, `col-span-1`, `col-span-2`, `col-span-3`, `col-span-4`, `col-span-5`, `col-span-6`, `col-span-7`, `col-span-8`, `col-span-9`, `col-span-10`, `col-span-11`, `col-span-12`, `col-span-full`

**Column Start:**
`col-start-1`, `col-start-2`, `col-start-3`, `col-start-4`, `col-start-5`, `col-start-6`, `col-start-7`, `col-start-8`, `col-start-9`, `col-start-10`, `col-start-11`, `col-start-12`, `col-start-13`, `col-start-auto`

**Column End:**
`col-end-1`, `col-end-2`, `col-end-3`, `col-end-4`, `col-end-5`, `col-end-6`, `col-end-7`, `col-end-8`, `col-end-9`, `col-end-10`, `col-end-11`, `col-end-12`, `col-end-13`, `col-end-auto`

**Row Span:**
`row-auto`, `row-span-1`, `row-span-2`, `row-span-3`, `row-span-4`, `row-span-5`, `row-span-6`, `row-span-full`

**Row Start:**
`row-start-1`, `row-start-2`, `row-start-3`, `row-start-4`, `row-start-5`, `row-start-6`, `row-start-7`, `row-start-auto`

**Row End:**
`row-end-1`, `row-end-2`, `row-end-3`, `row-end-4`, `row-end-5`, `row-end-6`, `row-end-7`, `row-end-auto`

### Flexbox

**Flex Direction:**
`flex-row`, `flex-row-reverse`, `flex-col`, `flex-col-reverse`

**Flex Wrap:**
`flex-wrap`, `flex-wrap-reverse`, `flex-nowrap`

**Flex:**
`flex-1`, `flex-auto`, `flex-initial`, `flex-none`

**Flex Grow:**
`grow`, `grow-0`

**Flex Shrink:**
`shrink`, `shrink-0`

**Flex Basis:**
`basis-0`, `basis-1`, `basis-2`, `basis-3`, `basis-4`, `basis-5`, `basis-6`, `basis-8`, `basis-10`, `basis-12`, `basis-16`, `basis-20`, `basis-24`, `basis-32`, `basis-40`, `basis-48`, `basis-56`, `basis-64`, `basis-72`, `basis-80`, `basis-96`, `basis-auto`, `basis-full`, `basis-1/2`, `basis-1/3`, `basis-2/3`, `basis-1/4`, `basis-2/4`, `basis-3/4`, `basis-1/5`, `basis-2/5`, `basis-3/5`, `basis-4/5`, `basis-1/6`, `basis-2/6`, `basis-3/6`, `basis-4/6`, `basis-5/6`, `basis-1/12`, `basis-2/12`, `basis-3/12`, `basis-4/12`, `basis-5/12`, `basis-6/12`, `basis-7/12`, `basis-8/12`, `basis-9/12`, `basis-10/12`, `basis-11/12`

**Order:**
`order-1`, `order-2`, `order-3`, `order-4`, `order-5`, `order-6`, `order-7`, `order-8`, `order-9`, `order-10`, `order-11`, `order-12`, `order-first`, `order-last`, `order-none`

### Alignment

**Place Content:**
`place-content-center`, `place-content-start`, `place-content-end`, `place-content-between`, `place-content-around`, `place-content-evenly`, `place-content-stretch`

**Place Items:**
`place-items-start`, `place-items-center`, `place-items-end`, `place-items-stretch`

**Place Self:**
`place-self-auto`, `place-self-start`, `place-self-center`, `place-self-end`, `place-self-stretch`

**Align Items:**
`items-start`, `items-center`, `items-end`, `items-stretch`, `items-baseline`

**Align Content:**
`content-start`, `content-center`, `content-end`, `content-between`, `content-around`, `content-evenly`, `content-stretch`

**Align Self:**
`self-auto`, `self-start`, `self-center`, `self-end`, `self-stretch`, `self-baseline`

**Justify Content:**
`justify-start`, `justify-center`, `justify-end`, `justify-between`, `justify-around`, `justify-evenly`

**Justify Items:**
`justify-items-start`, `justify-items-center`, `justify-items-end`, `justify-items-stretch`

**Justify Self:**
`justify-self-auto`, `justify-self-start`, `justify-self-center`, `justify-self-end`, `justify-self-stretch`

### Gap & Space Between

**Gap:**
`gap-0`, `gap-1`, `gap-2`, `gap-3`, `gap-4`, `gap-5`, `gap-6`, `gap-8`, `gap-10`, `gap-12`, `gap-16`, `gap-20`, `gap-24`

**Gap X:**
`gap-x-0`, `gap-x-1`, `gap-x-2`, `gap-x-3`, `gap-x-4`, `gap-x-5`, `gap-x-6`, `gap-x-8`, `gap-x-10`, `gap-x-12`, `gap-x-16`, `gap-x-20`, `gap-x-24`

**Gap Y:**
`gap-y-0`, `gap-y-1`, `gap-y-2`, `gap-y-3`, `gap-y-4`, `gap-y-5`, `gap-y-6`, `gap-y-8`, `gap-y-10`, `gap-y-12`, `gap-y-16`, `gap-y-20`, `gap-y-24`

**Space X:**
`space-x-0`, `space-x-1`, `space-x-2`, `space-x-3`, `space-x-4`, `space-x-5`, `space-x-6`, `space-x-8`, `space-x-10`, `space-x-12`, `space-x-16`, `space-x-20`, `space-x-24`, `space-x-reverse`

**Space Y:**
`space-y-0`, `space-y-1`, `space-y-2`, `space-y-3`, `space-y-4`, `space-y-5`, `space-y-6`, `space-y-8`, `space-y-10`, `space-y-12`, `space-y-16`, `space-y-20`, `space-y-24`, `space-y-reverse`

### Padding

**All Sides:**
`p-0`, `p-0.5`, `p-1`, `p-1.5`, `p-2`, `p-2.5`, `p-3`, `p-3.5`, `p-4`, `p-5`, `p-6`, `p-8`, `p-10`, `p-12`, `p-16`, `p-20`, `p-24`

**Horizontal:**
`px-0`, `px-0.5`, `px-1`, `px-1.5`, `px-2`, `px-2.5`, `px-3`, `px-3.5`, `px-4`, `px-5`, `px-6`, `px-8`, `px-10`, `px-12`, `px-16`, `px-20`, `px-24`

**Vertical:**
`py-0`, `py-0.5`, `py-1`, `py-1.5`, `py-2`, `py-2.5`, `py-3`, `py-3.5`, `py-4`, `py-5`, `py-6`, `py-8`, `py-10`, `py-12`, `py-16`, `py-20`, `py-24`

**Top:**
`pt-0`, `pt-0.5`, `pt-1`, `pt-1.5`, `pt-2`, `pt-2.5`, `pt-3`, `pt-3.5`, `pt-4`, `pt-5`, `pt-6`, `pt-8`, `pt-10`, `pt-12`, `pt-16`, `pt-20`, `pt-24`

**Right:**
`pr-0`, `pr-0.5`, `pr-1`, `pr-1.5`, `pr-2`, `pr-2.5`, `pr-3`, `pr-3.5`, `pr-4`, `pr-5`, `pr-6`, `pr-8`, `pr-10`, `pr-12`, `pr-16`, `pr-20`, `pr-24`

**Bottom:**
`pb-0`, `pb-0.5`, `pb-1`, `pb-1.5`, `pb-2`, `pb-2.5`, `pb-3`, `pb-3.5`, `pb-4`, `pb-5`, `pb-6`, `pb-8`, `pb-10`, `pb-12`, `pb-16`, `pb-20`, `pb-24`

**Left:**
`pl-0`, `pl-0.5`, `pl-1`, `pl-1.5`, `pl-2`, `pl-2.5`, `pl-3`, `pl-3.5`, `pl-4`, `pl-5`, `pl-6`, `pl-8`, `pl-10`, `pl-12`, `pl-16`, `pl-20`, `pl-24`

### Margin

**All Sides:**
`m-0`, `m-0.5`, `m-1`, `m-1.5`, `m-2`, `m-2.5`, `m-3`, `m-3.5`, `m-4`, `m-5`, `m-6`, `m-8`, `m-10`, `m-12`, `m-16`, `m-20`, `m-24`, `m-auto`

**Horizontal:**
`mx-0`, `mx-0.5`, `mx-1`, `mx-1.5`, `mx-2`, `mx-2.5`, `mx-3`, `mx-3.5`, `mx-4`, `mx-5`, `mx-6`, `mx-8`, `mx-10`, `mx-12`, `mx-16`, `mx-20`, `mx-24`, `mx-auto`

**Vertical:**
`my-0`, `my-0.5`, `my-1`, `my-1.5`, `my-2`, `my-2.5`, `my-3`, `my-3.5`, `my-4`, `my-5`, `my-6`, `my-8`, `my-10`, `my-12`, `my-16`, `my-20`, `my-24`, `my-auto`

**Top:**
`mt-0`, `mt-0.5`, `mt-1`, `mt-1.5`, `mt-2`, `mt-2.5`, `mt-3`, `mt-3.5`, `mt-4`, `mt-5`, `mt-6`, `mt-8`, `mt-10`, `mt-12`, `mt-16`, `mt-20`, `mt-24`, `mt-auto`

**Right:**
`mr-0`, `mr-0.5`, `mr-1`, `mr-1.5`, `mr-2`, `mr-2.5`, `mr-3`, `mr-3.5`, `mr-4`, `mr-5`, `mr-6`, `mr-8`, `mr-10`, `mr-12`, `mr-16`, `mr-20`, `mr-24`, `mr-auto`

**Bottom:**
`mb-0`, `mb-0.5`, `mb-1`, `mb-1.5`, `mb-2`, `mb-2.5`, `mb-3`, `mb-3.5`, `mb-4`, `mb-5`, `mb-6`, `mb-8`, `mb-10`, `mb-12`, `mb-16`, `mb-20`, `mb-24`, `mb-auto`

**Left:**
`ml-0`, `ml-0.5`, `ml-1`, `ml-1.5`, `ml-2`, `ml-2.5`, `ml-3`, `ml-3.5`, `ml-4`, `ml-5`, `ml-6`, `ml-8`, `ml-10`, `ml-12`, `ml-16`, `ml-20`, `ml-24`, `ml-auto`

### Width

**Fixed Width:**
`w-0`, `w-1`, `w-2`, `w-3`, `w-4`, `w-5`, `w-6`, `w-8`, `w-10`, `w-12`, `w-16`, `w-20`, `w-24`, `w-32`, `w-40`, `w-48`, `w-56`, `w-64`, `w-72`, `w-80`, `w-96`

**Relative Width:**
`w-auto`, `w-1/2`, `w-1/3`, `w-2/3`, `w-1/4`, `w-3/4`, `w-1/5`, `w-2/5`, `w-3/5`, `w-4/5`, `w-full`, `w-screen`, `w-min`, `w-max`, `w-fit`

**Min Width:**
`min-w-0`, `min-w-full`, `min-w-min`, `min-w-max`, `min-w-fit`

**Max Width:**
`max-w-xs`, `max-w-sm`, `max-w-md`, `max-w-lg`, `max-w-xl`, `max-w-2xl`, `max-w-3xl`, `max-w-4xl`, `max-w-5xl`, `max-w-6xl`, `max-w-7xl`, `max-w-full`, `max-w-prose`

### Height

**Fixed Height:**
`h-0`, `h-1`, `h-2`, `h-3`, `h-4`, `h-5`, `h-6`, `h-8`, `h-10`, `h-12`, `h-16`, `h-20`, `h-24`, `h-32`, `h-40`, `h-48`, `h-56`, `h-64`, `h-72`, `h-80`, `h-96`

**Relative Height:**
`h-auto`, `h-1/2`, `h-1/3`, `h-2/3`, `h-1/4`, `h-3/4`, `h-1/5`, `h-2/5`, `h-3/5`, `h-4/5`, `h-full`, `h-screen`, `h-min`, `h-max`, `h-fit`

**Min Height:**
`min-h-0`, `min-h-full`, `min-h-screen`, `min-h-min`, `min-h-max`, `min-h-fit`

**Max Height:**
`max-h-0`, `max-h-full`, `max-h-screen`, `max-h-min`, `max-h-max`, `max-h-fit`

### Typography - Font Size

`text-xs`, `text-sm`, `text-base`, `text-lg`, `text-xl`, `text-2xl`, `text-3xl`, `text-4xl`, `text-5xl`, `text-6xl`, `text-7xl`, `text-8xl`, `text-9xl`

### Typography - Font Weight

`font-thin`, `font-extralight`, `font-light`, `font-normal`, `font-medium`, `font-semibold`, `font-bold`, `font-extrabold`, `font-black`

### Typography - Font Style

`italic`, `not-italic`

### Typography - Text Alignment

`text-left`, `text-center`, `text-right`, `text-justify`

### Typography - Text Decoration

`underline`, `line-through`, `no-underline`

### Typography - Text Transform

`uppercase`, `lowercase`, `capitalize`, `normal-case`

### Typography - Text Overflow

`truncate`, `text-ellipsis`, `text-clip`

### Typography - Line Height

`leading-none`, `leading-tight`, `leading-snug`, `leading-normal`, `leading-relaxed`, `leading-loose`, `leading-3`, `leading-4`, `leading-5`, `leading-6`, `leading-7`, `leading-8`, `leading-9`, `leading-10`

### Typography - Letter Spacing

`tracking-tighter`, `tracking-tight`, `tracking-normal`, `tracking-wide`, `tracking-wider`, `tracking-widest`

### Typography - Vertical Alignment

`align-baseline`, `align-top`, `align-middle`, `align-bottom`, `align-text-top`, `align-text-bottom`

### Typography - Whitespace

`whitespace-normal`, `whitespace-nowrap`, `whitespace-pre`, `whitespace-pre-line`, `whitespace-pre-wrap`

### Typography - Word Break

`break-normal`, `break-words`, `break-all`

### Typography - List Style

`list-none`, `list-disc`, `list-decimal`, `list-inside`, `list-outside`

### Text Colors

**Primary:**
`text-primary`, `text-primary-50`, `text-primary-100`, `text-primary-200`, `text-primary-300`, `text-primary-400`, `text-primary-500`, `text-primary-600`, `text-primary-700`, `text-primary-800`, `text-primary-900`

**Secondary:**
`text-secondary`, `text-secondary-50`, `text-secondary-100`, `text-secondary-200`, `text-secondary-300`, `text-secondary-400`, `text-secondary-500`, `text-secondary-600`, `text-secondary-700`, `text-secondary-800`, `text-secondary-900`

**Accent:**
`text-accent`, `text-accent-50`, `text-accent-100`, `text-accent-200`, `text-accent-300`, `text-accent-400`, `text-accent-500`, `text-accent-600`, `text-accent-700`, `text-accent-800`, `text-accent-900`

**Info:**
`text-info`, `text-info-50`, `text-info-100`, `text-info-200`, `text-info-300`, `text-info-400`, `text-info-500`, `text-info-600`, `text-info-700`, `text-info-800`, `text-info-900`

**Success:**
`text-success`, `text-success-50`, `text-success-100`, `text-success-200`, `text-success-300`, `text-success-400`, `text-success-500`, `text-success-600`, `text-success-700`, `text-success-800`, `text-success-900`

**Warning:**
`text-warning`, `text-warning-50`, `text-warning-100`, `text-warning-200`, `text-warning-300`, `text-warning-400`, `text-warning-500`, `text-warning-600`, `text-warning-700`, `text-warning-800`, `text-warning-900`

**Error:**
`text-error`, `text-error-50`, `text-error-100`, `text-error-200`, `text-error-300`, `text-error-400`, `text-error-500`, `text-error-600`, `text-error-700`, `text-error-800`, `text-error-900`

**Surface:**
`text-surface`, `text-surface-50`, `text-surface-100`, `text-surface-200`, `text-surface-300`, `text-surface-400`, `text-surface-500`, `text-surface-600`, `text-surface-700`, `text-surface-800`, `text-surface-900`

**Text:**
`text-text`, `text-text-50`, `text-text-100`, `text-text-200`, `text-text-300`, `text-text-400`, `text-text-500`, `text-text-600`, `text-text-700`, `text-text-800`, `text-text-900`

**Background:**
`text-bg`, `text-bg-50`, `text-bg-100`, `text-bg-200`, `text-bg-300`, `text-bg-400`, `text-bg-500`, `text-bg-600`, `text-bg-700`, `text-bg-800`, `text-bg-900`

**Special:**
`text-on-surface`, `text-white`, `text-black`, `text-transparent`, `text-current`

### Background Colors

**Primary:**
`bg-primary`, `bg-primary-50`, `bg-primary-100`, `bg-primary-200`, `bg-primary-300`, `bg-primary-400`, `bg-primary-500`, `bg-primary-600`, `bg-primary-700`, `bg-primary-800`, `bg-primary-900`

**Secondary:**
`bg-secondary`, `bg-secondary-50`, `bg-secondary-100`, `bg-secondary-200`, `bg-secondary-300`, `bg-secondary-400`, `bg-secondary-500`, `bg-secondary-600`, `bg-secondary-700`, `bg-secondary-800`, `bg-secondary-900`

**Accent:**
`bg-accent`, `bg-accent-50`, `bg-accent-100`, `bg-accent-200`, `bg-accent-300`, `bg-accent-400`, `bg-accent-500`, `bg-accent-600`, `bg-accent-700`, `bg-accent-800`, `bg-accent-900`

**Info:**
`bg-info`, `bg-info-50`, `bg-info-100`, `bg-info-200`, `bg-info-300`, `bg-info-400`, `bg-info-500`, `bg-info-600`, `bg-info-700`, `bg-info-800`, `bg-info-900`

**Success:**
`bg-success`, `bg-success-50`, `bg-success-100`, `bg-success-200`, `bg-success-300`, `bg-success-400`, `bg-success-500`, `bg-success-600`, `bg-success-700`, `bg-success-800`, `bg-success-900`

**Warning:**
`bg-warning`, `bg-warning-50`, `bg-warning-100`, `bg-warning-200`, `bg-warning-300`, `bg-warning-400`, `bg-warning-500`, `bg-warning-600`, `bg-warning-700`, `bg-warning-800`, `bg-warning-900`

**Error:**
`bg-error`, `bg-error-50`, `bg-error-100`, `bg-error-200`, `bg-error-300`, `bg-error-400`, `bg-error-500`, `bg-error-600`, `bg-error-700`, `bg-error-800`, `bg-error-900`

**Surface:**
`bg-surface`, `bg-surface-50`, `bg-surface-100`, `bg-surface-200`, `bg-surface-300`, `bg-surface-400`, `bg-surface-500`, `bg-surface-600`, `bg-surface-700`, `bg-surface-800`, `bg-surface-900`

**Text:**
`bg-text`, `bg-text-50`, `bg-text-100`, `bg-text-200`, `bg-text-300`, `bg-text-400`, `bg-text-500`, `bg-text-600`, `bg-text-700`, `bg-text-800`, `bg-text-900`

**Background:**
`bg-bg`, `bg-bg-50`, `bg-bg-100`, `bg-bg-200`, `bg-bg-300`, `bg-bg-400`, `bg-bg-500`, `bg-bg-600`, `bg-bg-700`, `bg-bg-800`, `bg-bg-900`

**Special:**
`bg-surface`, `bg-white`, `bg-black`, `bg-transparent`, `bg-current`

### Border Colors

**Primary:**
`border-primary`, `border-primary-50`, `border-primary-100`, `border-primary-200`, `border-primary-300`, `border-primary-400`, `border-primary-500`, `border-primary-600`, `border-primary-700`, `border-primary-800`, `border-primary-900`

**Secondary:**
`border-secondary`, `border-secondary-50`, `border-secondary-100`, `border-secondary-200`, `border-secondary-300`, `border-secondary-400`, `border-secondary-500`, `border-secondary-600`, `border-secondary-700`, `border-secondary-800`, `border-secondary-900`

**Accent:**
`border-accent`, `border-accent-50`, `border-accent-100`, `border-accent-200`, `border-accent-300`, `border-accent-400`, `border-accent-500`, `border-accent-600`, `border-accent-700`, `border-accent-800`, `border-accent-900`

**Info:**
`border-info`, `border-info-50`, `border-info-100`, `border-info-200`, `border-info-300`, `border-info-400`, `border-info-500`, `border-info-600`, `border-info-700`, `border-info-800`, `border-info-900`

**Success:**
`border-success`, `border-success-50`, `border-success-100`, `border-success-200`, `border-success-300`, `border-success-400`, `border-success-500`, `border-success-600`, `border-success-700`, `border-success-800`, `border-success-900`

**Warning:**
`border-warning`, `border-warning-50`, `border-warning-100`, `border-warning-200`, `border-warning-300`, `border-warning-400`, `border-warning-500`, `border-warning-600`, `border-warning-700`, `border-warning-800`, `border-warning-900`

**Error:**
`border-error`, `border-error-50`, `border-error-100`, `border-error-200`, `border-error-300`, `border-error-400`, `border-error-500`, `border-error-600`, `border-error-700`, `border-error-800`, `border-error-900`

**Surface:**
`border-surface`, `border-surface-50`, `border-surface-100`, `border-surface-200`, `border-surface-300`, `border-surface-400`, `border-surface-500`, `border-surface-600`, `border-surface-700`, `border-surface-800`, `border-surface-900`

**Text:**
`border-text`, `border-text-50`, `border-text-100`, `border-text-200`, `border-text-300`, `border-text-400`, `border-text-500`, `border-text-600`, `border-text-700`, `border-text-800`, `border-text-900`

**Background:**
`border-bg`, `border-bg-50`, `border-bg-100`, `border-bg-200`, `border-bg-300`, `border-bg-400`, `border-bg-500`, `border-bg-600`, `border-bg-700`, `border-bg-800`, `border-bg-900`

**Special:**
`border-border`, `border-white`, `border-black`, `border-transparent`, `border-current`

### Border Width

**All Sides:**
`border`, `border-0`, `border-2`, `border-4`, `border-8`

**X Axis:**
`border-x`, `border-x-0`, `border-x-2`, `border-x-4`, `border-x-8`

**Y Axis:**
`border-y`, `border-y-0`, `border-y-2`, `border-y-4`, `border-y-8`

**Top:**
`border-t`, `border-t-0`, `border-t-2`, `border-t-4`, `border-t-8`

**Right:**
`border-r`, `border-r-0`, `border-r-2`, `border-r-4`, `border-r-8`

**Bottom:**
`border-b`, `border-b-0`, `border-b-2`, `border-b-4`, `border-b-8`

**Left:**
`border-l`, `border-l-0`, `border-l-2`, `border-l-4`, `border-l-8`

### Border Style

`border-solid`, `border-dashed`, `border-dotted`, `border-double`, `border-none`

### Border Radius

**All Corners:**
`rounded-none`, `rounded-sm`, `rounded`, `rounded-md`, `rounded-lg`, `rounded-xl`, `rounded-2xl`, `rounded-3xl`, `rounded-full`

**Top:**
`rounded-t-none`, `rounded-t-sm`, `rounded-t`, `rounded-t-md`, `rounded-t-lg`, `rounded-t-xl`, `rounded-t-2xl`, `rounded-t-3xl`, `rounded-t-full`

**Right:**
`rounded-r-none`, `rounded-r-sm`, `rounded-r`, `rounded-r-md`, `rounded-r-lg`, `rounded-r-xl`, `rounded-r-2xl`, `rounded-r-3xl`, `rounded-r-full`

**Bottom:**
`rounded-b-none`, `rounded-b-sm`, `rounded-b`, `rounded-b-md`, `rounded-b-lg`, `rounded-b-xl`, `rounded-b-2xl`, `rounded-b-3xl`, `rounded-b-full`

**Left:**
`rounded-l-none`, `rounded-l-sm`, `rounded-l`, `rounded-l-md`, `rounded-l-lg`, `rounded-l-xl`, `rounded-l-2xl`, `rounded-l-3xl`, `rounded-l-full`

### Shadows

**Light Mode:**
`shadow-none`, `shadow-sm`, `shadow`, `shadow-md`, `shadow-lg`, `shadow-xl`, `shadow-2xl`, `shadow-inner`

**Dark Mode:**
`dark:shadow-none`, `dark:shadow-sm`, `dark:shadow`, `dark:shadow-md`, `dark:shadow-lg`, `dark:shadow-xl`, `dark:shadow-2xl`, `dark:shadow-inner`

### Opacity

**Light Mode:**
`opacity-0`, `opacity-5`, `opacity-10`, `opacity-20`, `opacity-25`, `opacity-30`, `opacity-40`, `opacity-50`, `opacity-60`, `opacity-70`, `opacity-75`, `opacity-80`, `opacity-90`, `opacity-95`, `opacity-100`

**Dark Mode:**
`dark:opacity-0`, `dark:opacity-5`, `dark:opacity-10`, `dark:opacity-20`, `dark:opacity-25`, `dark:opacity-30`, `dark:opacity-40`, `dark:opacity-50`, `dark:opacity-60`, `dark:opacity-70`, `dark:opacity-75`, `dark:opacity-80`, `dark:opacity-90`, `dark:opacity-95`, `dark:opacity-100`

### Filters - Blur

`blur-none`, `blur-sm`, `blur`, `blur-md`, `blur-lg`, `blur-xl`, `blur-2xl`, `blur-3xl`

### Filters - Brightness

`brightness-0`, `brightness-50`, `brightness-75`, `brightness-90`, `brightness-95`, `brightness-100`, `brightness-105`, `brightness-110`, `brightness-125`, `brightness-150`, `brightness-200`

### Filters - Contrast

`contrast-0`, `contrast-50`, `contrast-75`, `contrast-100`, `contrast-125`, `contrast-150`, `contrast-200`

### Filters - Grayscale

`grayscale-0`, `grayscale`

### Filters - Invert

`invert-0`, `invert`

### Filters - Sepia

`sepia-0`, `sepia`

### Transforms - Rotate

`rotate-0`, `rotate-1`, `rotate-2`, `rotate-3`, `rotate-6`, `rotate-12`, `rotate-45`, `rotate-90`, `rotate-180`, `-rotate-1`, `-rotate-2`, `-rotate-3`, `-rotate-6`, `-rotate-12`, `-rotate-45`, `-rotate-90`, `-rotate-180`

### Transforms - Scale

`scale-0`, `scale-50`, `scale-75`, `scale-90`, `scale-95`, `scale-100`, `scale-105`, `scale-110`, `scale-125`, `scale-150`

### Transitions

**Transition Property:**
`transition`, `transition-none`, `transition-all`, `transition-colors`, `transition-opacity`, `transition-shadow`, `transition-transform`

**Duration:**
`duration-75`, `duration-100`, `duration-150`, `duration-200`, `duration-300`, `duration-500`, `duration-700`, `duration-1000`

**Timing:**
`ease-linear`, `ease-in`, `ease-out`, `ease-in-out`

### Interactivity

**Cursor:**
`cursor-auto`, `cursor-default`, `cursor-pointer`, `cursor-wait`, `cursor-text`, `cursor-move`, `cursor-not-allowed`

**Pointer Events:**
`pointer-events-none`, `pointer-events-auto`

**User Select:**
`select-none`, `select-text`, `select-all`, `select-auto`

**Resize:**
`resize-none`, `resize`, `resize-y`, `resize-x`

### Overflow

**Overflow:**
`overflow-auto`, `overflow-hidden`, `overflow-visible`, `overflow-scroll`

**Overflow X:**
`overflow-x-auto`, `overflow-x-hidden`, `overflow-x-visible`, `overflow-x-scroll`

**Overflow Y:**
`overflow-y-auto`, `overflow-y-hidden`, `overflow-y-visible`, `overflow-y-scroll`

### Background

**Background Attachment:**
`bg-fixed`, `bg-local`, `bg-scroll`

**Background Clip:**
`bg-clip-border`, `bg-clip-padding`, `bg-clip-content`, `bg-clip-text`

**Background Repeat:**
`bg-repeat`, `bg-no-repeat`, `bg-repeat-x`, `bg-repeat-y`

### Object Fit & Position

**Object Fit:**
`object-contain`, `object-cover`, `object-fill`, `object-none`, `object-scale-down`

**Object Position:**
`object-bottom`, `object-center`, `object-left`, `object-left-bottom`, `object-left-top`, `object-right`, `object-right-bottom`, `object-right-top`, `object-top`

### Tables

**Border Collapse:**
`border-collapse`, `border-separate`

**Table Layout:**
`table-auto`, `table-fixed`

### Miscellaneous

**Appearance:**
`appearance-none`

**Scroll Behavior:**
`scroll-auto`, `scroll-smooth`

---

## Quick Reference Tips

### Common Patterns

**Card with shadow:**
```
{{div class="p-6 bg-white rounded-lg shadow-md"}}
  Card content
{{/div}}
```

**Centered container:**
```
{{div class="max-w-4xl mx-auto p-8"}}
  Centered content with max width
{{/div}}
```

**Button-like appearance:**
```
{{div class="px-4 py-2 bg-primary text-white rounded cursor-pointer"}}
  Click me
{{/div}}
```

**Two-column layout:**
```
{{div class="@container"}}
  {{div class="grid @md:grid-cols-2 gap-6"}}
    {{div}}Column 1{{/div}}
    {{div}}Column 2{{/div}}
  {{/div}}
{{/div}}
```

**Flex spacing:**
```
{{div class="flex items-center gap-4"}}
  {{span}}Item 1{{/span}}
  {{span}}Item 2{{/span}}
  {{span}}Item 3{{/span}}
{{/div}}
```

### Spacing Scale

The spacing scale follows a consistent pattern:
- `0` = 0
- `0.5` = 0.125rem (2px)
- `1` = 0.25rem (4px)
- `2` = 0.5rem (8px)
- `3` = 0.75rem (12px)
- `4` = 1rem (16px)
- `5` = 1.25rem (20px)
- `6` = 1.5rem (24px)
- `8` = 2rem (32px)
- `10` = 2.5rem (40px)
- `12` = 3rem (48px)
- `16` = 4rem (64px)
- `20` = 5rem (80px)
- `24` = 6rem (96px)
