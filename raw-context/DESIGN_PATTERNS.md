# Nebari About Page - Design Pattern Reference

Quick reference for the redesigned About page design patterns. Use this for consistency when building other pages or making updates.

---

## Typography Patterns

### Oversized Headlines

**Usage:** Hero sections, major announcements
```tsx
<h1 className="text-5xl sm:text-6xl md:text-7xl lg:text-8xl font-bold leading-[1.1] tracking-tight">
  Building Strong Roots
</h1>
```

**Key features:**
- Responsive scale (48px → 96px)
- Tight leading (1.1)
- Tight tracking
- Bold weight (700)

### Section Headlines

**Usage:** Section titles throughout page
```tsx
<h2 className="text-4xl md:text-5xl lg:text-6xl font-bold mb-8">
  Our Mission
</h2>
```

### Eyebrow Text

**Usage:** Small labels above headlines
```tsx
<div className="flex items-center gap-3 mb-6">
  <div className="w-12 h-px bg-accent" />
  <span className="text-sm font-medium tracking-wider uppercase text-muted-foreground">
    Our Name
  </span>
</div>
```

### Accent Text Inline

**Usage:** Highlighted words in headlines
```tsx
<span className="relative inline-block">
  <span className="relative z-10">South Africa's</span>
  {/* Accent underline */}
  <span className="absolute bottom-2 left-0 w-full h-4 bg-accent/30 -rotate-1" />
</span>
```

---

## Geometric Pattern Library

### Concentric Circles (Growth Rings)

**Usage:** Hero background, decorative elements
```tsx
<div className="absolute top-0 right-0 w-[600px] h-[600px] opacity-[0.03]">
  <svg viewBox="0 0 600 600" className="w-full h-full">
    <circle cx="300" cy="300" r="280" fill="none" stroke="currentColor" strokeWidth="40" />
    <circle cx="300" cy="300" r="180" fill="none" stroke="currentColor" strokeWidth="30" />
    <circle cx="300" cy="300" r="100" fill="none" stroke="currentColor" strokeWidth="20" />
  </svg>
</div>
```

**Key features:**
- 3 concentric circles
- Opacity: 3-5%
- Absolute positioned
- Pointer-events-none

### Grid Pattern Overlay

**Usage:** CTA sections, textured backgrounds
```tsx
<div className="absolute inset-0 opacity-[0.02]">
  <svg width="100%" height="100%">
    <defs>
      <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
        <path d="M 40 0 L 0 0 0 40" fill="none" stroke="white" strokeWidth="1" />
      </pattern>
    </defs>
    <rect width="100%" height="100%" fill="url(#grid)" />
  </svg>
</div>
```

**Key features:**
- 40x40 grid
- Opacity: 2%
- SVG pattern
- Full viewport coverage

### Corner Brackets

**Usage:** Framing elements, accent decorations
```tsx
{/* Top-left */}
<div className="absolute top-0 left-0 w-20 h-20 border-l-4 border-t-4 border-accent/40" />

{/* Bottom-right */}
<div className="absolute bottom-0 right-0 w-20 h-20 border-r-4 border-b-4 border-accent/40" />

{/* With rounded corners */}
<div className="absolute top-8 right-8 w-16 h-16 border-r-2 border-t-2 border-accent/30 rounded-tr-2xl" />
```

**Key features:**
- 2-4px borders
- Accent color 30-40%
- Corner only (L or ⌐ shapes)
- Optional rounded corners

### Accent Lines & Dividers

**Usage:** Section separators, eyebrow decorations
```tsx
{/* Horizontal gradient line */}
<div className="w-16 h-px bg-gradient-to-r from-transparent to-accent" />

{/* Full-width with dots */}
<div className="flex items-center gap-4">
  <div className="w-16 h-px bg-gradient-to-r from-transparent to-accent" />
  <div className="w-2 h-2 rounded-full bg-accent" />
  <div className="flex-1 h-px bg-gradient-to-l from-transparent to-accent" />
</div>

{/* Vertical accent */}
<div className="absolute -left-6 top-0 bottom-0 w-1 bg-accent/40" />
```

### Organic Shapes

**Usage:** Abstract bonsai-inspired decorations
```tsx
<div className="absolute bottom-10 right-10 w-40 h-40 opacity-[0.04]">
  <svg viewBox="0 0 200 200" className="w-full h-full">
    <path
      d="M100,20 Q140,60 120,100 T100,180 Q60,140 80,100 T100,20"
      fill="currentColor"
    />
  </svg>
</div>
```

---

## Color Patterns

### Background Gradients

**Subtle Section Background:**
```tsx
className="bg-gradient-to-br from-secondary/40 via-background to-accent/5"
```

**Dark Section Background:**
```tsx
<div className="absolute inset-0 bg-gradient-to-br from-foreground via-foreground/95 to-foreground/90" />
```

**Accent Overlay (on dark):**
```tsx
<div className="absolute top-0 right-0 w-2/3 h-full bg-gradient-to-bl from-accent/30 via-accent/10 to-transparent" />
```

### Card Gradients

**Lime Green Variant:**
```tsx
className="bg-gradient-to-br from-accent/10 to-accent/5"
```

**Gray Variant:**
```tsx
className="bg-gradient-to-br from-foreground/5 to-foreground/10"
```

**Success Green Variant:**
```tsx
className="bg-gradient-to-br from-success/10 to-success/5"
```

**Muted Variant:**
```tsx
className="bg-gradient-to-br from-muted/50 to-muted/30"
```

### Accent Applications

**High emphasis (40%):**
```tsx
className="bg-accent/40"  // Underlines, active states
```

**Medium emphasis (20-30%):**
```tsx
className="bg-accent/20"  // Icon backgrounds, hover states
```

**Low emphasis (5-10%):**
```tsx
className="bg-accent/10"  // Card backgrounds, subtle highlights
```

**Ultra-subtle (3-5%):**
```tsx
className="opacity-[0.03]"  // Geometric patterns, watermarks
```

---

## Card Patterns

### Icon Feature Card

**Usage:** Gallery, Features sections
```tsx
<Card className="group relative overflow-hidden border-2 hover:shadow-xl transition-all duration-300">
  {/* Background gradient */}
  <div className="absolute inset-0 bg-gradient-to-br from-accent/10 to-accent/5 opacity-50 group-hover:opacity-100 transition-opacity" />

  {/* Content */}
  <CardContent className="relative p-8 md:p-10">
    {/* Icon */}
    <div className="mb-6">
      <div className="inline-flex p-4 rounded-xl bg-accent/20 shadow-lg group-hover:scale-110 transition-transform">
        <Icon className="w-8 h-8 text-accent-dark" strokeWidth={1.5} />
      </div>
    </div>

    {/* Title */}
    <h3 className="text-2xl md:text-3xl font-semibold mb-4">
      {title}
    </h3>

    {/* Description */}
    <p className="text-muted-foreground text-lg leading-relaxed">
      {description}
    </p>

    {/* Decorative watermark icon */}
    <div className="absolute bottom-8 right-8 w-20 h-20 opacity-[0.05] group-hover:opacity-[0.1] transition-opacity pointer-events-none">
      <Icon className="w-full h-full" strokeWidth={1} />
    </div>

    {/* Accent corner on hover */}
    <div className="absolute top-0 right-0 w-16 h-16 border-r-2 border-t-2 border-accent/30 rounded-tr-xl opacity-0 group-hover:opacity-100 transition-opacity" />
  </CardContent>
</Card>
```

**Key features:**
- Group hover effects
- Gradient background (50% → 100%)
- Icon scales on hover (1.0 → 1.1)
- Watermark icon increases opacity
- Corner bracket appears on hover

### Value Prop Card

**Usage:** Mission section, feature highlights
```tsx
<div className="p-6 rounded-xl bg-gradient-to-br from-accent/5 to-accent/10 border border-accent/20">
  <div className="text-3xl font-bold text-foreground mb-2">
    For Everyone
  </div>
  <p className="text-sm text-muted-foreground">
    From beginners to masters
  </p>
</div>
```

### Glassmorphism Card (Optional)

**Usage:** Overlays on dark backgrounds
```tsx
<div className="bg-white/10 backdrop-blur-md rounded-2xl p-8 md:p-12 border border-white/20">
  {content}
</div>
```

---

## Icon Patterns

### Icon in Circle

**Usage:** Section icons, bullet points
```tsx
<div className="w-10 h-10 rounded-lg bg-accent/10 flex items-center justify-center">
  <Target className="w-5 h-5 text-accent-dark" />
</div>
```

**Sizes:**
- Small: `w-8 h-8` with `w-4 h-4` icon
- Medium: `w-10 h-10` with `w-5 h-5` icon
- Large: `w-12 h-12` with `w-6 h-6` icon

### Icon Badge

**Usage:** Labels, tags, status indicators
```tsx
<div className="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-accent/20 backdrop-blur-sm border border-accent/30">
  <Sparkles className="w-4 h-4 text-accent" />
  <span className="text-sm font-medium text-foreground">
    Join the Movement
  </span>
</div>
```

### Icon Watermark

**Usage:** Decorative background element
```tsx
<div className="absolute bottom-8 right-8 w-20 h-20 opacity-[0.05] group-hover:opacity-[0.1] transition-opacity pointer-events-none">
  <Icon className="w-full h-full" strokeWidth={1} />
</div>
```

---

## Animation Patterns

### Entrance Animations (Staggered)

**Usage:** Hero elements, sequential reveals
```tsx
<div className="animate-fade-in">First element</div>
<div className="animate-fade-in animation-delay-100">Second element</div>
<div className="animate-fade-in animation-delay-200">Third element</div>
<div className="animate-fade-in animation-delay-300">Fourth element</div>
```

**Available delays:**
- `animation-delay-100` (100ms)
- `animation-delay-200` (200ms)
- `animation-delay-300` (300ms)
- `animation-delay-400` (400ms)

### Float Animation

**Usage:** Scroll indicators, decorative elements
```tsx
<div className="animate-float">
  <ArrowDown className="w-6 h-6" />
</div>
```

### Hover Transforms

**Scale on hover:**
```tsx
className="group-hover:scale-110 transition-transform duration-300"
```

**Translate on hover:**
```tsx
className="group-hover:translate-x-1 transition-transform duration-300"
```

**Opacity change:**
```tsx
className="opacity-50 group-hover:opacity-100 transition-opacity duration-300"
```

### Button Hover Effect

**Usage:** Animated background on buttons
```tsx
<Button className="group relative overflow-hidden">
  <span className="relative z-10">Button Text</span>
  <span className="absolute inset-0 bg-accent/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300" />
</Button>
```

---

## Layout Patterns

### Asymmetric Split Layout

**Usage:** Content + visual element
```tsx
<div className="grid lg:grid-cols-12 gap-12">
  {/* Content - larger (7 columns) */}
  <div className="lg:col-span-7 order-2 lg:order-1">
    {content}
  </div>

  {/* Visual - smaller (5 columns) */}
  <div className="lg:col-span-5 order-1 lg:order-2">
    {visual}
  </div>
</div>
```

### Centered Content Block

**Usage:** Hero, CTA sections
```tsx
<div className="max-w-6xl mx-auto">
  <div className="text-center">
    {content}
  </div>
</div>
```

### Card Grid

**2-column:**
```tsx
<div className="grid md:grid-cols-2 gap-8 max-w-6xl mx-auto">
  {cards}
</div>
```

**3-column:**
```tsx
<div className="grid md:grid-cols-3 gap-8 max-w-6xl mx-auto">
  {cards}
</div>
```

**4-column (responsive 2x2):**
```tsx
<div className="grid sm:grid-cols-2 gap-8 max-w-6xl mx-auto">
  {cards}
</div>
```

---

## Spacing System

### Section Spacing

**Standard sections:**
```tsx
className="py-20 md:py-24"
```

**Major sections (hero, CTA):**
```tsx
className="py-24 md:py-32"
```

### Element Spacing

**Small gap:** `mb-4` or `gap-4` (16px)
**Medium gap:** `mb-6` or `gap-6` (24px)
**Large gap:** `mb-8` or `gap-8` (32px)
**Extra large gap:** `mb-12` or `gap-12` (48px)
**Section gap:** `mb-16` or `gap-16` (64px)

### Container Padding

```tsx
className="container mx-auto px-4"
```

---

## Responsive Patterns

### Typography Scaling

**Hero headlines:**
```tsx
text-5xl sm:text-6xl md:text-7xl lg:text-8xl
```

**Section headlines:**
```tsx
text-4xl md:text-5xl lg:text-6xl
```

**Body text:**
```tsx
text-lg md:text-xl
```

### Layout Stacking

**Desktop split, mobile stack:**
```tsx
<div className="grid lg:grid-cols-2 gap-12">
  <div className="order-2 lg:order-1">Left on desktop</div>
  <div className="order-1 lg:order-2">Right on desktop, top on mobile</div>
</div>
```

### Hide on Mobile

```tsx
className="hidden md:block"  // Show tablet+
className="hidden lg:block"  // Show desktop only
```

---

## Accessibility Patterns

### Focus States

**Buttons:**
```tsx
className="focus:ring-2 focus:ring-accent focus:outline-none"
```

**Links:**
```tsx
className="focus-visible:outline-accent focus-visible:outline-2"
```

### Color Contrast

**Accent text on white:**
```tsx
className="text-accent-accessible"  // WCAG AA compliant
```

**Body text:**
```tsx
className="text-muted-foreground"  // 4.5:1 contrast
```

### Semantic HTML

**Section structure:**
```tsx
<section aria-labelledby="section-heading">
  <h2 id="section-heading">Heading</h2>
  {content}
</section>
```

**Decorative elements:**
```tsx
<div aria-hidden="true">
  {decorative patterns}
</div>
```

---

## Common Combinations

### Eyebrow + Headline + Description

```tsx
<div>
  {/* Eyebrow */}
  <div className="flex items-center gap-3 mb-6">
    <div className="w-12 h-px bg-accent" />
    <span className="text-sm font-medium tracking-wider uppercase text-muted-foreground">
      Label
    </span>
  </div>

  {/* Headline */}
  <h2 className="text-4xl md:text-5xl font-bold mb-6">
    Main Heading
  </h2>

  {/* Description */}
  <p className="text-lg md:text-xl text-muted-foreground leading-relaxed">
    Description text goes here.
  </p>
</div>
```

### Icon Bullet List

```tsx
<div className="space-y-6">
  <div className="flex gap-4">
    <div className="shrink-0 mt-1">
      <div className="w-10 h-10 rounded-lg bg-accent/10 flex items-center justify-center">
        <Icon className="w-5 h-5 text-accent-dark" />
      </div>
    </div>
    <p className="text-lg text-muted-foreground leading-relaxed">
      List item text
    </p>
  </div>
</div>
```

### Decorative Dot Sequence

```tsx
<div className="flex items-center gap-2">
  <div className="w-2 h-2 rounded-full bg-accent animate-pulse" />
  <div className="w-2 h-2 rounded-full bg-accent animate-pulse animation-delay-100" />
  <div className="w-2 h-2 rounded-full bg-accent animate-pulse animation-delay-200" />
</div>
```

---

## Performance Best Practices

### Lazy Loading Below-Fold Content

```tsx
<section style={{ contentVisibility: "auto" }}>
  {belowFoldContent}
</section>
```

### Optimize SVG

- Keep viewBox simple
- Use currentColor for fills
- Minimize path complexity
- Inline small SVGs (< 2KB)

### Minimize Re-renders

```tsx
export const Component = memo(function Component() {
  // Component logic
});
```

---

## Don'ts (Anti-patterns)

### Typography

❌ `text-8xl` on all screen sizes (too large on mobile)
✓ `text-5xl sm:text-6xl md:text-7xl lg:text-8xl`

❌ Long lines without max-width
✓ `max-w-3xl mx-auto` for readable line lengths

### Colors

❌ `bg-accent` on large areas (overwhelming)
✓ `bg-accent/10` for backgrounds

❌ Accent text without `accent-text` token
✓ Use `text-accent-accessible` for contrast

### Animations

❌ Auto-playing animations
✓ Hover-triggered animations

❌ Motion without `prefers-reduced-motion` check
✓ Respect user preferences (browser handles with Tailwind)

### Layout

❌ Fixed widths
✓ Responsive with max-width containers

❌ Absolute positioning without parent context
✓ Relative parent with absolute children

---

**Reference Version:** 1.0
**Last Updated:** 2026-01-19
**For:** Nebari About Page Redesign

Use these patterns consistently across the About page for cohesive design.
