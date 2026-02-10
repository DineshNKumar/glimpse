# Stats Section Design - Glimpse

## Overview

A modern, card-based statistics section has been implemented across the **landing page**, **login page**, and **signup page**, showcasing impressive metrics that build trust and credibility with users. Each page has a unique design optimized for its context.

## Implementation Date

February 10, 2026

## Statistics Displayed

### 1. Files Shared

- **Value**: 10M+
- **Icon**: `file-text`
- **Purpose**: Demonstrates platform usage and reliability

### 2. Active Users

- **Value**: 500K+
- **Icon**: `users`
- **Purpose**: Shows community size and engagement

### 3. Uptime

- **Value**: 99.9%
- **Icon**: `activity`
- **Purpose**: Highlights system reliability and stability

### 4. Encryption

- **Value**: 256-bit
- **Icon**: `shield`
- **Purpose**: Emphasizes security and data protection

## Design Variations by Page

### Landing Page Design (Main Homepage)

**Visual Style:**

- **Layout**: Responsive grid (4 columns on desktop, 2 on mobile)
- **Card Style**: White cards with gradient borders
- **Background**: Pure white with subtle shadow
- **Border**: 2px solid gray with hover transition to primary color
- **Border Radius**: 2xl rounded corners

**Key Features:**

- **Gradient Icon Containers**: 64x64px with primary gradient background
- **Gradient Text Values**: Text uses gradient fill for premium look
- **Larger Icons**: 28px icons for better visibility
- **Section Heading**: "Trusted by thousands worldwide"
- **Prominent Shadows**: Elevated cards with purple-tinted shadows on hover
- **Lift Effect**: Cards translate up 4px on hover

**Color Scheme:**

- Card background: White
- Border: Gray → Primary purple on hover
- Icon background: Linear gradient (primary-500 → primary-600)
- Text value: Gradient text (primary-500 → primary-700)
- Shadow: Purple-tinted (rgba(103, 126, 234, 0.15))

### Authentication Pages Design (Login & Signup)

**Visual Style:**

- **Layout**: 2x2 grid
- **Card Style**: Glass-morphism with frosted effect
- **Background**: `rgba(255, 255, 255, 0.1)` with backdrop blur
- **Border**: Subtle white border at 15% opacity
- **Border Radius**: Extra large rounded corners

**Key Features:**

- **Glass Effect**: Transparent with backdrop blur
- **Smaller Icons**: 24px icons
- **Section Heading**: "Trusted by thousands"
- **Subtle Lift**: Cards translate up 2px on hover
- **White-on-gradient**: Works on purple gradient background

**Color Scheme:**

- Card background: rgba(255, 255, 255, 0.1)
- Border: White at 15-25% opacity
- Icon background: White at 15% opacity
- Text: All white with opacity variations
- Designed for dark/gradient backgrounds

## Design Features

### Visual Design

- **Layout**: 2x2 grid of cards
- **Card Style**: Glass-morphism with frosted glass effect
- **Background**: `rgba(255, 255, 255, 0.1)` with backdrop blur
- **Border**: Subtle white border at 15% opacity
- **Border Radius**: Extra large rounded corners (`var(--radius-xl)`)

### Interactive States

- **Hover Effect**:
  - Background lightens to 15% opacity
  - Border brightens to 25% opacity
  - Card lifts 2px upward (`translateY(-2px)`)
  - Smooth transition animation

### Typography

- **Title**: "Trusted by thousands"
  - Size: `var(--text-lg)`
  - Weight: Bold
  - Color: White
  - Alignment: Center

- **Stat Value**:
  - Size: `var(--text-3xl)` (large and prominent)
  - Weight: Black (900)
  - Color: White
  - Line height: 1 (tight for impact)

- **Stat Label**:
  - Size: `var(--text-sm)`
  - Weight: Medium
  - Color: White with 85% opacity

### Icon Design

- **Container**: 48x48px circular background
- **Background**: White at 15% opacity
- **Border Radius**: Extra large for soft appearance
- **Icon Size**: 24px
- **Color**: White
- **Positioning**: Centered above stat value

## Code Structure

### HTML Structure

```svelte
<div class="auth-stats">
	<h3 class="stats-title">Trusted by thousands</h3>
	<div class="stats-grid">
		<div class="stat-card">
			<div class="stat-icon">
				<Icon name="file-text" size={24} />
			</div>
			<div class="stat-value">10M+</div>
			<div class="stat-label">Files Shared</div>
		</div>
		<!-- ... more cards ... -->
	</div>
</div>
```

### CSS Classes

#### `.auth-stats`

- Container for the entire stats section
- Adds top margin and padding
- Includes subtle top border for visual separation

#### `.stats-title`

- Section heading
- Large, bold, centered text
- White color with proper spacing

#### `.stats-grid`

- CSS Grid with 2 columns
- Equal-sized cards
- Consistent gap spacing

#### `.stat-card`

- Individual stat container
- Glass-morphism effect
- Hover interactions
- Smooth transitions

#### `.stat-icon`

- Icon container with circular background
- Centered positioning
- Consistent sizing

#### `.stat-value`

- Large, bold numbers
- High visual prominence
- Tight line height

#### `.stat-label`

- Descriptive text below value
- Smaller size with medium weight
- Slightly transparent

## Pages Implemented

### 1. Landing Page (`/` - Homepage)

- **Location**: Features section, after feature cards
- **Design**: Premium white cards with gradient accents
- **Heading**: "Trusted by thousands worldwide"
- **Layout**: Responsive 4-column grid (2 columns on mobile)
- **Style**: Solid white cards with gradient icons and text
- **Purpose**: First impression, build immediate trust with visitors
- **Shadow**: Purple-tinted elevation on hover
- **Border**: Transitions from gray to purple on hover

### 2. Signup Page (`/signup`)

- **Location**: Between benefits list and testimonial
- **Design**: Glass-morphism cards on gradient background
- **Heading**: "Trusted by thousands"
- **Layout**: 2x2 grid
- **Style**: Frosted glass effect with white transparency
- **Purpose**: Reinforce platform credibility during account creation
- **Context**: Part of left-side branding content

### 3. Login Page (`/login`)

- **Location**: Below features list in left-side branding
- **Design**: Glass-morphism cards on gradient background
- **Heading**: "Trusted by thousands"
- **Layout**: 2x2 grid
- **Style**: Frosted glass effect with white transparency
- **Purpose**: Welcome back returning users with confidence metrics
- **Context**: Part of authentication branding area
- Purpose: Showcase platform value and encourage signups

## Design Rationale

### Why This Design?

1. **Glass-morphism**: Modern, premium aesthetic that aligns with the Glimpse brand
2. **Grid Layout**: Clean, organized presentation of multiple metrics
3. **Iconography**: Visual reinforcement of each statistic's meaning
4. **Hover Effects**: Subtle interactivity that engages users
5. **Consistency**: Same design across login and signup for cohesive experience

### User Experience Benefits

✅ **Trust Building**: Large numbers create immediate credibility  
✅ **Visual Hierarchy**: Icons → Values → Labels creates clear reading order  
✅ **Scannability**: Grid layout allows quick information absorption  
✅ **Professional Feel**: Glass effect and smooth animations suggest quality  
✅ **Reassurance**: Security and reliability metrics reduce user anxiety

## Technical Details

### Responsive Behavior

- Grid adapts to container width
- Cards maintain aspect ratio
- Text scales appropriately
- Icons remain centered

### Performance

- Minimal CSS (lightweight)
- Hardware-accelerated transforms
- Efficient backdrop blur
- No JavaScript required

### Accessibility

- Semantic HTML structure
- Proper heading hierarchy
- Clear label-value relationships
- Color contrast maintained

## Color Palette

### Background Colors

- **Card Base**: `rgba(255, 255, 255, 0.1)`
- **Card Hover**: `rgba(255, 255, 255, 0.15)`
- **Icon Background**: `rgba(255, 255, 255, 0.15)`

### Border Colors

- **Card Border**: `rgba(255, 255, 255, 0.15)`
- **Card Hover Border**: `rgba(255, 255, 255, 0.25)`
- **Section Border**: `rgba(255, 255, 255, 0.1)`

### Text Colors

- All text: White (`#ffffff`)
- Label opacity: 85%

## Future Enhancements

### Potential Additions

- [ ] Animated counter on page load
- [ ] Real-time updating stats (if connected to API)
- [ ] Additional metrics (response time, countries served, etc.)
- [ ] Tooltip explanations for each stat
- [ ] Animation when scrolling into view

### A/B Testing Opportunities

- Different stat combinations
- Alternative layouts (horizontal, single row)
- Icon vs. no icon variations
- Value formatting styles

## Implementation Files

### Modified Files

1. `/src/routes/signup/+page.svelte`
   - Added stats HTML structure
   - Added CSS styles for stats section

2. `/src/routes/login/+page.svelte`
   - Replaced old simple stats with new card design
   - Updated CSS to match signup page
   - Added encryption metric (was missing)

3. `/src/routes/index/+page.svelte`
   - Implemented new stats section design
   - Adapted layout for landing page context

### Icons Used

- `file-text` - Document/file representation
- `users` - Community/people icon
- `activity` - Graph/activity representation
- `shield` - Security/protection symbol

All icons from the Glimpse icon system.

## Maintenance Notes

### Updating Statistics

To update the displayed values, edit the HTML in both login and signup pages:

```svelte
<div class="stat-value">NEW_VALUE</div>
```

### Adding New Stats

1. Add a new `.stat-card` div to the grid
2. Consider grid layout (may need to adjust to 3 or 4 columns)
3. Choose appropriate icon from icon library
4. Ensure responsive behavior is maintained

### Styling Adjustments

All styles are defined in the `<style>` section of each page. Look for:

- `.auth-stats` - Main container
- `.stats-grid` - Grid configuration
- `.stat-card` - Individual card styles

## Conclusion

The stats section effectively communicates Glimpse's scale, reliability, and security through clean, modern design. The glass-morphism aesthetic aligns with the overall brand identity while providing an engaging, interactive experience that builds user confidence during the authentication process.

**Status**: ✅ Implemented and tested  
**Version**: 1.0.0  
**Last Updated**: February 10, 2026
