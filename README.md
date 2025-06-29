# WDYW - What Do You Want?

A smart restaurant picker app that helps you and your significant other decide where to eat in Traverse City, with intelligent weighting based on your preferences, time of day, and effort level.

## Features

- **Smart Selection**: Weighted random selection based on multiple factors
- **Time-Aware**: Automatically detects breakfast, lunch, or dinner time
- **Preference-Based**: Filter by sit-down vs takeout and effort level
- **Live Status**: Shows if restaurants are currently open or closed
- **GitHub Pages Ready**: Works as a static site with no backend required

## How It Works

### 1. Service Type Preference

Choose between three options:
- **No Preference**: Considers all restaurants equally
- **Sit-down**: Strongly favors dine-in restaurants
- **Takeout**: Strongly favors takeout/quick service options

**Weighting:**
- Matching preference: **2x weight** (twice as likely to be selected)
- Non-matching: **0.1x weight** (10 times less likely to be selected)
- No preference: **1x weight** (normal likelihood)

### 2. Effort Level (1-5 Scale)

Set your maximum effort level using the slider:
- **1**: Quick & Easy (fast food, grab & go)
- **2**: Low Effort (counter service, quick casual)
- **3**: Moderate (casual dining, some wait)
- **4**: Higher Effort (nice dinner, reservation recommended)
- **5**: Special Occasion (fine dining, plan ahead)

**Weighting:**
- Within your effort range: **1x weight** (normal likelihood)
- Above your effort range: **0.2x weight** (5 times less likely)

### 3. Meal Time Preferences (0-5 Scale)

Each restaurant has a preference score for breakfast, lunch, and dinner:
- **0**: Not suitable for this meal (closed or inappropriate)
- **1**: Poor choice for this meal
- **2**: Below average choice
- **3**: Decent option for this meal
- **4**: Good choice for this meal
- **5**: Excellent/Perfect for this meal

**Time Ranges:**
- Breakfast: 5:00 AM - 11:00 AM
- Lunch: 11:00 AM - 4:00 PM
- Dinner: 4:00 PM - 5:00 AM

**Weighting:**
- The meal score is added to 1, then multiplied with the weight
- Score 0 → 1x multiplier (minimal boost)
- Score 3 → 4x multiplier (good boost)
- Score 5 → 6x multiplier (maximum boost)

## Example Weight Calculation

Let's say it's 7:00 PM (dinner time) and you want takeout with maximum effort level 2:

**Jersey Mike's Subs:**
- Base weight: 1
- Service: Takeout ✓ → 2x weight = 2
- Effort: Level 1 (within limit) → 1x = 2
- Dinner score: 4 → multiply by 5 = **10 total weight**

**Jolly Pumpkin:**
- Base weight: 1
- Service: Sit-down only ✗ → 0.1x weight = 0.1
- Effort: Level 4 (exceeds limit) → 0.2x = 0.02
- Dinner score: 5 → multiply by 6 = **0.12 total weight**

Jersey Mike's would be about 83x more likely to be selected in this scenario!

## Restaurant Data Structure

Each restaurant in `restaurants.json` includes:

```json
{
  "id": 1,
  "name": "Restaurant Name",
  "cuisine": "Type of Food",
  "price": "$-$$$",
  "phone": "(231) 555-1234",
  "address": "123 Main St, Traverse City, MI",
  "hours": {
    "monday": { "open": "11:00", "close": "21:00" },
    "tuesday": null,  // Closed this day
    // ... other days
  },
  "service": ["sitdown", "takeout"],  // Available service types
  "effortLevel": 3,  // 1-5 scale
  "mealPreference": {
    "breakfast": 0,
    "lunch": 4,
    "dinner": 5
  }
}
```

## Customizing Restaurants

To add or modify restaurants, edit `restaurants.json`:

1. **Adding a restaurant**: Copy an existing entry and update all fields
2. **Adjusting preferences**: Modify the `mealPreference` scores based on your actual preferences
3. **Changing effort levels**: Update `effortLevel` based on how much planning/time/money the restaurant requires
4. **Service types**: Use `["sitdown"]`, `["takeout"]`, or `["sitdown", "takeout"]`

## Technical Notes

- Uses vanilla JavaScript with no dependencies
- Fetches restaurant data asynchronously
- Responsive design works on mobile and desktop
- Handles GitHub Pages subdirectory routing automatically
- Falls back gracefully if no restaurants match criteria

## Future Enhancement Ideas

- Add cuisine type filtering
- Include price range preferences
- Integration with Google Maps APIs for real-time hours
- Restaurant Lookup with Google Maps APIs for quick adding
- Add simple accounts and database to go wider than just us
- If "Roll Again" is selected, remove the current selection from the next randomized roll IE we've decided we don't want that, don't offer it again this session
- Adjust meal time weighting so a "0" for certain meal time means there's no chance it's selected

## Contributing

Feel free to submit issues or pull requests to improve the app!
