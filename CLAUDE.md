project:
  name: ABC Travels
  type: Static Website
  description: A travel agency landing page showcasing popular destinations, about info, and contact/newsletter sections.

files:
  - file: index.html
    purpose: Main HTML structure of the website
    sections:
      - id: header
        tag: header
        content: Site title "ABC Travels"

      - id: navigation
        tag: nav
        features:
          - Hamburger toggle button (☰)
          - Slide-in sidebar with links to Destinations, About, Contact
          - Toggle logic via touchstart event listener (mobile-only, no click handler)
        issues:
          - touchstart only — desktop clicks won't open the nav
          - No closing mechanism (click outside or button)
          - Script is inline and placed before the nav content renders fully

      - id: destinations
        tag: section.destinations
        cards:
          - name: New York City
            price: "$1,999 (7 Days)"
            offer: Free airport transfer
            image_src: travelandleisure.com (external URL)
          - name: Paris
            price: "$2,499 (10 Days)"
            offer: Seine River cruise upgrade for $99
            image_src: travelandleisure.com (external URL)
          - name: Tokyo
            price: "$1,799 (5 Days)"
            offer: Traditional tea ceremony included
            image_src: wallpapers.com (external URL)
        issues:
          - Images rely on external URLs (can break anytime)
          - No responsive grid layout (cards stack vertically only)
          - No "Book Now" CTA button on any card

      - id: about
        tag: section#about
        content: Description of ABC Travels as a travel agency

      - id: contact
        tag: section#contact
        details:
          phone: 123-456-7890
          email: info@abctravels.com
          address: 123 Main St, Anytown USA

      - id: subscribe
        tag: section#subscribe
        parent: contact section (nested, not sibling — structural issue)
        form:
          input: email (required)
          button: Subscribe
        issues:
          - form action is "#" (does nothing on submit)
          - No success/error feedback to user
          - Nested inside contact section instead of being its own section

    missing_elements:
      - Closing </section> and </body>, </html> tags
      - Footer element entirely absent
      - No meta viewport tag (breaks mobile responsiveness)
      - No favicon

  - file: style1.css
    purpose: Styling for the entire website
    css_variables:
      primary_color: "#388e3c (Forest Green)"
      accent_color: "#ff9900 (Orange)"
      text_color: "#333 (Dark Gray)"
      bg_color: "#f5f5f5 (Light Gray)"
      font_family: Arial, sans-serif
      heading_font: Lora, serif

    styles_defined:
      - selector: body
        notes: Sets font, background, and line-height (1.8)

      - selector: header
        notes: Green-to-orange gradient background, white text, centered

      - selector: nav / "#navigation"
        notes: >
          Fixed sidebar, 200px wide, full viewport height.
          DUPLICATE definition — #navigation is declared twice with conflicting rules.
          translateX(-100%) hides it; .active class slides it in.

      - selector: ".destination"
        notes: Flex column card with shadow, hover scale transform

      - selector: ".destination img"
        notes: Full width, hover zoom (scale 1.03) and opacity reduction

      - selector: ".destination:hover::after"
        notes: Dark overlay on hover — uses position:absolute but parent has no position:relative, so overlay won't align correctly

      - selector: "#toggleNav"
        notes: Absolute positioned button, top-right, font-size 24px

      - selector: footer
        notes: Styled (green bg, fixed bottom) but no footer tag exists in HTML

      - selector: "#contactDetails"
        notes: Hidden by default, shown when .active — but no element with id="contactDetails" exists in HTML

    issues:
      - "#navigation styles defined twice with conflicting properties"
      - ".active #navigation vs #navigation.active — selector mismatch causes toggle to not work"
      - "footer CSS defined but no <footer> in HTML"
      - "#contactDetails CSS defined but no matching HTML element"
      - "No media queries — layout breaks on mobile/tablet"
      - "No dark mode support"
      - ".destination has no position:relative, breaking the ::after overlay"

overall_issues:
  critical:
    - Nav toggle only works on touch (mobile), broken on desktop
    - Active class selector mismatch (.active #navigation vs #navigation.active)
    - HTML file is missing closing tags (unclosed sections, no </body> or </html>)
    - No meta viewport tag — mobile layout will not render correctly

  ui_ux:
    - No responsive grid for destination cards (should be 3-column on desktop)
    - No CTA buttons on destination cards
    - Subscribe form gives no feedback on submission
    - Nav has no close/dismiss mechanism

  code_quality:
    - Duplicate CSS selectors for #navigation
    - Dead CSS rules (footer, #contactDetails) with no matching HTML
    - Inline script using touchstart only
    - External image URLs are fragile and slow

improvement_opportunities:
  - Add meta viewport tag for mobile support
  - Fix nav toggle to work on both click and touch
  - Fix .active selector mismatch in CSS
  - Add responsive CSS grid for destination cards (1 col mobile, 3 col desktop)
  - Add "Book Now" button to each destination card
  - Add form submission feedback (success message or basic JS validation)
  - Remove duplicate #navigation CSS block
  - Add a proper <footer> element to match existing footer CSS
  - Add position:relative to .destination to fix hover overlay
  - Close all open HTML tags properly