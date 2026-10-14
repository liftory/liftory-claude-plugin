# The menu document

create_nutrition_plan and validate_nutrition_plan take one whole menu; get_last_nutrition_plan and get_nutrition_plan return the same shape, so a menu can be read, edited, and submitted again. The draft has no ids and no positions: order is the order of the lists, and every cross reference is by name.

- **Menu:** name, activate, rules (the notes printed under the menu, as markdown), targets (daily kcal, protein, carbohydrate, and fat in grams; omitted when the menu prescribes none), plannedDays (day variants with their days per week, which total 7 when there are any), slots, weeklyLimits.
- **Slot:** one meal of the day, in the order of the day: name, an optional time hint as HH:mm, and a note printed under the meal that applies whichever option is taken.
- **Option:** one alternative for that meal: name, a prepNote about the whole option, and an optional plannedDay name when the option applies only on that day variant. Do not name an option like its meal.
- **Group:** a run of lines inside an option. chooseOne false means all of it is eaten; chooseOne true means one line is picked. category is protein, carb, fat, fruit, vegetable, or other, and can be left empty so Liftory infers it from the foods. A group note speaks about that run.
- **Item:** one food line: name, foodId, amount (free text, stored as written), grams (only when the amount is an exact weight), optional, preferred, and a note about that one food. Foods eaten together as one choice are one item with components, each with its own name, foodId, amount, and grams.
- **Weekly limit:** name, maxPerWeek, the slots it applies to (empty means every meal), and the options or items it counts, each referenced by slot, option, and item name.

What the result means:
- unknown lists foodIds the catalog rejected, and problems lists references that match nothing or several things, empty names, and bad times. When either is non-empty, nothing was saved.
- warnings never block: they name lines saved without a foodId (shown, but counted in no figures) and one amount priced two different ways.
- activate defaults to true and archives the person's other menus. Set it explicitly to what the user chose.
