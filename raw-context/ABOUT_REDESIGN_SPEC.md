# About Page Redesign Specification

## Design Philosophy: "Geometric Growth"

### Core Principles

1. **No background images on hero/CTA sections** - User's primary concern
2. **Minimal image dependency** - Reduce from 7+ images to 0-1 strategic images
3. **Bold typography as hero element** - Text is the primary visual
4. **Geometric patterns** - Inspired by bonsai branch structure and roots
5. **Strategic accent usage** - Lime green (#84cc16) for impact
6. **Performance-first** - Following Vercel React best practices

---

## Visual Design System

### Typography Hierarchy

**Hero Headlines**
- Size: `text-5xl` to `text-8xl` (responsive)
- Weight: `font-bold` (700)
- Leading: `leading-[1.1]` (tight)
- Tracking: `tracking-tight`
- Purpose: Create impact without imagery

**Section Headlines**
- Size: `text-4xl` to `text-6xl`
- Weight: `font-bold`
- Purpose: Strong visual hierarchy

**Body Text**
- Size: `text-lg` to `text-xl`
- Color: `text-muted-foreground`
- Leading: `leading-relaxed`
- Purpose: Readable, accessible content

### Color Strategy

**Background Gradients** (instead of images)
```css
/* Subtle mesh gradient */
bg-gradient-to-br from-secondary/40 via-background to-accent/5

/* Dark section with accent */
bg-gradient-to-br from-foreground via-foreground/95 to-foreground/90
+ overlay: from-accent/30 via-accent/10 to-transparent
```

**Accent Applications**
- Underlines and highlights
- Border accents
- Geometric shape fills
- Button hover states
- Gradient overlays (10-30% opacity)

### Geometric Elements

**Pattern Library**
1. **Concentric circles** - Growth rings (hero)
2. **Corner brackets** - Framing elements
3. **Accent lines** - Horizontal/vertical dividers
4. **Grid patterns** - Subtle texture (2% opacity)
5. **Abstract organic shapes** - Bonsai-inspired forms

**Implementation**
- Pure CSS where possible (borders, gradients)
- Inline SVG for complex shapes (lightweight)
- Opacity: 2-5% for background elements
- 20-40% for interactive elements

### Spacing & Layout

**Asymmetric Layouts**
- 7/5 column split for content-first sections
- Offset headings with decorative elements
- Strategic whitespace (py-24 to py-32)

**Container Widths**
- Hero: `max-w-6xl`
- Content sections: `max-w-6xl` to `max-w-7xl`
- Text-focused: `max-w-3xl`

---

## Component Specifications

### 1. Hero Section (REDESIGNED)

**Current Issues**
- Full-screen background image (70vh)
- Image-dependent for visual impact

**New Design**
- Clean gradient background
- Oversized typography (text-8xl max)
- Geometric accent shapes (SVG circles, lines)
- Layered text effect with background text
- Accent underline on key words
- Stats bar at bottom (no images needed)
- Height: min-h-[85vh]

**Key Features**
- Eyebrow text with line accent
- 3-line bold headline with inline accent
- Geometric shapes in background (3-5% opacity)
- Scroll indicator (optional)
- Stats: 9 Provinces | 100% Local | 1 Community

**Performance**
- No image loading
- Pure CSS animations
- Lazy-load stats section if needed

### 2. CTA Section (REDESIGNED)

**Current Issues**
- Full background image
- Heavy reliance on photography

**New Design**
- Dark background (foreground color)
- Lime green gradient overlay (20-30% opacity)
- Geometric shapes for texture
- Grid pattern overlay (2% opacity)
- Bold typography as focal point
- Icon badge above headline

**Key Features**
- Icon badge: "Join the Movement"
- Large headline (text-7xl)
- Prominent dual CTAs
- Trust indicators (text-based, no images)
- Corner bracket accents

**Color Palette**
```
Background: foreground (dark)
Accent gradient: accent/30 to accent/10
Text: background (white on dark)
Buttons: accent (primary), outline (secondary)
```

### 3. Why Nebari Section (REDESIGNED)

**Current Issues**
- Large background image

**New Design**
- Gradient background (no image)
- Split layout: 50/50 or 40/60
- Left: Abstract illustration (SVG/icon-based)
- Right: Content with strong hierarchy
- Japanese character (根) as decorative element

**Visual Element (replacing photo)**
- Large TreePine icon (w-64 h-64, 10% opacity)
- Japanese characters for visual interest
- Geometric frame with corner accents
- Gradient background (accent/10 to accent/5)

### 4. Gallery Section (REDESIGNED)

**Current Issues**
- 3 bonsai photos

**New Design**
- Remove photos entirely
- Replace with 3 illustrated feature cards
- Icon-based visual hierarchy
- Gradient backgrounds per card
- Hover effects for interactivity

**Card Structure**
- Large icon (TreePine, Leaf, Sprout)
- Gradient background (different per card)
- Title + description
- Decorative icon watermark (5% opacity)
- Border with hover state

### 5. Mission Section (REDUCED IMAGES)

**Current Issues**
- Large side image

**New Design Options**

**Option A: Keep 1 small image**
- Reduce to aspect-square
- Smaller column (5/12 instead of 6/12)
- Content takes priority (7/12)
- Accent gradient overlay on image

**Option B: Remove image entirely**
- Replace with geometric composition
- Large icon with gradient
- Abstract shape background
- Text-based value props

### 6. Features Section (RECOMMENDATIONS)

**Current State**
- 2 large images + 2 cards

**Recommended Changes**
- Remove both feature images
- Convert all to uniform cards (4 total)
- Icon-based visual hierarchy
- Consistent layout (2x2 grid on desktop)
- Gradient backgrounds (varied)

---

## Implementation Guide

### File Structure

```
src/components/about/
├── HeroSection.redesign.tsx          ✓ Created
├── CTASection.redesign.tsx           ✓ Created
├── WhyNebariSection.redesign.tsx     ✓ Created
├── GallerySection.redesign.tsx       ✓ Created
├── MissionSection.redesign.tsx       ✓ Created
└── [other sections - use as-is or update]
```

### Integration Steps

1. **Test individual components**
   - Import redesigned components
   - Verify content props compatibility
   - Test responsive behavior

2. **Update About page**
   - Replace old imports with `.redesign` versions
   - Test full page layout
   - Verify animations work

3. **Performance validation**
   - Run Lighthouse audit
   - Check Core Web Vitals
   - Verify mobile performance

4. **A/B test (optional)**
   - Keep old components temporarily
   - Feature flag to switch between versions
   - Gather user feedback

### Content Requirements

Ensure `@/content/about` exports include:
- `HeroContent` with title, subtitle, cta
- `CTAContent` with heading, subtitle, primaryCTA, secondaryCTA
- `WhyNebariContent` with heading, definition, explanation
- `GalleryContent` with heading
- `MissionContent` with heading, mission1, mission2, image (optional)

---

## Responsive Behavior

### Mobile (< 768px)
- Typography: text-5xl to text-6xl
- Single column layouts
- Stack stats horizontally
- Reduce geometric complexity
- Hide decorative elements if needed

### Tablet (768px - 1024px)
- Typography: text-6xl to text-7xl
- 2-column grids for cards
- Show most geometric elements
- Maintain spacing ratios

### Desktop (> 1024px)
- Full typography scale (text-8xl)
- Asymmetric layouts (7/5 splits)
- All decorative elements visible
- Expanded spacing

---

## Accessibility

### WCAG 2.1 Compliance

**Color Contrast**
- Accent on white: Uses `accent-text` (75 75% 32%) - 4.5:1 ratio
- White on dark: > 15:1 ratio
- Muted text: > 4.5:1 ratio

**Focus States**
- All interactive elements have visible focus
- Button outlines use `ring-accent`
- Keyboard navigation supported

**Semantic HTML**
- `<section>` with `aria-labelledby`
- Proper heading hierarchy (h1 → h2 → h3)
- `<button>` and `<Link>` properly used

**Screen Reader Support**
- Alt text for any remaining images
- Icon decorations use `aria-hidden="true"` (add if needed)
- Skip links for long sections

---

## Performance Optimizations

### Vercel Best Practices

1. **No LCP images**
   - Hero has no image (instant LCP)
   - CTA has no image

2. **Minimal JavaScript**
   - Static components (no client-side logic)
   - CSS animations only
   - Lazy load below-fold sections

3. **CSS Optimization**
   - Tailwind purges unused classes
   - No custom CSS files
   - Utility-first approach

4. **Bundle Size**
   - Remove image imports (saves KB)
   - Inline SVG (< 1KB each)
   - Tree-shake unused icons

### Lighthouse Targets

- Performance: 95+
- Accessibility: 100
- Best Practices: 100
- SEO: 100

---

## Animation Strategy

### Entrance Animations

**Fade-in with slide**
```tsx
className="animate-fade-in"
className="animate-fade-in animation-delay-100"
```

**Use cases**
- Hero elements (staggered)
- Section content reveal
- CTA elements

**Float animation**
```tsx
className="animate-float"
```

**Use cases**
- Scroll indicators
- Decorative elements
- Accent dots

### Hover Effects

**Scale on hover**
```tsx
className="group-hover:scale-110 transition-transform"
```

**Translate on hover**
```tsx
className="group-hover:translate-x-1 transition-transform"
```

---

## Testing Checklist

- [ ] Hero section renders without images
- [ ] CTA section renders without images
- [ ] Typography is legible at all sizes
- [ ] Accent colors have sufficient contrast
- [ ] Geometric elements don't overwhelm content
- [ ] Animations are smooth (60fps)
- [ ] Mobile layout works without horizontal scroll
- [ ] Keyboard navigation works
- [ ] Screen reader announces sections properly
- [ ] Dark mode works (if enabled)
- [ ] Performance metrics meet targets

---

## Design Rationale

### Why Remove Images?

1. **User feedback** - Explicit request to remove hero/CTA backgrounds
2. **Performance** - Images are the #1 performance bottleneck
3. **Flexibility** - No photo sourcing dependency
4. **Modernity** - Bold typography is on-trend (see Stripe, Linear, Vercel)
5. **Brand consistency** - Text-first approach scales better

### Why Geometric Patterns?

1. **Bonsai metaphor** - Circles = growth rings, lines = branches
2. **Scalability** - SVG patterns work at any size
3. **Performance** - Inline SVG is tiny compared to images
4. **Uniqueness** - Custom patterns = brand differentiation
5. **Accessibility** - Decorative patterns don't need alt text

### Why Bold Typography?

1. **Visual hierarchy** - Text naturally guides eye
2. **Content-first** - Story is the hero, not imagery
3. **Mobile-friendly** - Text scales better than images
4. **Trust factor** - Professional typography = credibility
5. **Performance** - Text renders instantly

---

## Future Enhancements

### Phase 2 (Optional)

1. **Animated SVG illustrations**
   - Subtle line animations
   - Drawn-on-scroll effects
   - Lightweight GSAP integration

2. **Gradient mesh backgrounds**
   - CSS `conic-gradient` experiments
   - Animated gradient positions
   - More dynamic feel

3. **3D elements**
   - CSS 3D transforms
   - Parallax effects
   - Depth without images

4. **Micro-interactions**
   - Cursor-follow effects
   - Hover state variations
   - Button press feedback

---

## Migration Path

### Immediate (No breaking changes)
1. Deploy redesigned components as `.redesign.tsx`
2. Update About page to import redesigned versions
3. Test thoroughly
4. Deploy

### Future (After validation)
1. Remove old component files
2. Rename `.redesign.tsx` to `.tsx`
3. Clean up unused image imports
4. Update content structure if needed

---

**Created:** 2026-01-19
**Status:** Ready for Implementation
**Impact:** Major visual refresh, significant performance improvement
