# Casa (Styl-like) – Expo + React Native

Swipe-first fashion discovery. Right to like (wishlist), left to pass, up to add-to-cart. Long‑press opens a product detail modal. On-device recommender re-ranks items from your interactions, with exploration mixed-in.

• Expo SDK 51 • React Native 0.74 • expo-router • NativeWind (Tailwind) • Zustand • AsyncStorage

## Features
- Swipe Deck: left/right horizontal-only, swipe up to cart, LOVE/PASS overlays, undo last action
- Product Detail: long‑press to open modal with sizes/colors and quick actions
- Wishlist & Cart: Zustand stores with persistence; floating badges with counts
- Onboarding: pick categories; preferences saved; “Prefs” button on deck to revisit
- Recommender: weighted/decayed events (view/like/cart), profile vectors, exploration injection
- Admin skeleton (optional paths) for analytics/feed health

## Project Structure
```
app/
  _layout.tsx      # Navigation container & providers
  index.tsx        # Redirects to onboarding or deck based on saved prefs
  onboarding.tsx   # Category selection (saves to AsyncStorage)
  deck.tsx         # Swipe deck (gestures, modal, undo, badges, recommender)
  cart.tsx         # Cart screen
  wishlist.tsx     # Wishlist screen
  ...
assets/            # Images/fonts (placeholder)
src/
  components/
    CartBadge.tsx
    WishlistBadge.tsx
    ProfileBadge.tsx
  data/
    categories.ts
    items.ts
  lib/
    recommender.ts   # on-device recommendation logic
    gluestack.tsx    # stub UI provider (no external dep required)
  state/
    cart.ts
    wishlist.ts
    interactions.ts
    auth.ts
```

See docs/ARCHITECTURE.md for a deeper dive.

## Setup
1) Install dependencies
```bash
npm install
npx expo install
```

2) Start the app
```bash
npm run start
```
- Open on device with Expo Go (scan QR) or press `a` (Android) / `i` (iOS Simulator on macOS).

3) Platform helpers (optional)
```bash
npm run android
npm run ios
npm run web
```

## Usage Tips
- First launch shows onboarding. Pick one or more categories and continue.
- To change preferences later, tap “⚙︎ Prefs” in the deck header to revisit onboarding.
- Gestures:
  - Right = like (adds to wishlist)
  - Left = pass
  - Up = add to cart
  - Long‑press = product detail modal
- Undo: after an action, tap “↶ Undo” (removes from wishlist/cart if needed).

## Recommender Summary
- Events: view(1), like(3), cart(5), purchase(10) with 30‑min half‑life decay
- Profile vectors: tags, category, brand, colors (partial), price tier
- Ranking: score by profile + inject ~15% exploration from tail for diversity

## Troubleshooting
- Loading issues: The deck falls back to bundled items if the recommender is slow; you shouldn’t get stuck on loading. If you do, check device network or clear app cache.
- Hook errors: We ensure hook order is stable; if hot-reload shows a hook-order warning, full refresh fixes it.
- Gestures feel diagonal: Deck locks vertical translation for left/right; swipe up requires more intent to avoid misclassification.

## Scripts
- `npm run start`      Start Expo dev server
- `npm run android`    Build & run Android
- `npm run ios`        Build & run iOS (macOS)
- `npm run web`        Run in web
- `npm run typecheck`  TypeScript check

## License
Proprietary — internal project