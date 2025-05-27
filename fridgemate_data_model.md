
# 🧠 FridgeMate Data Model

---

## 🧍 User

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `id`                | UUID           | Unique user ID                                   |
| `name`              | String         | Display name                                     |
| `email`             | String         | For login or sharing purposes                    |
| `dietary_preferences` | Array[String] | e.g. ["vegetarian", "low-carb"]                  |
| `allergies`         | Array[String]  | e.g. ["peanuts", "gluten"]                       |
| `skill_level`       | Enum           | beginner, intermediate, advanced                 |
| `joined_at`         | Timestamp      | When user joined                                 |
| `house_id`          | UUID           | Foreign key to `House` table (optional)          |

---

## 🏠 House

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `id`                | UUID           | Unique house ID                                  |
| `name`              | String         | Name of the house                                |
| `members`           | Array[UUID]    | User IDs of house members                        |
| `storage_limit_liters` | Number      | Estimate of shared storage space                 |

---

## 📅 MealPlan

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `id`                | UUID           | Unique meal plan ID                              |
| `user_id`           | UUID           | Owner/creator of the plan                        |
| `house_id`          | UUID           | Optional - for shared plans                      |
| `start_date`        | Date           | Start of the plan week                           |
| `end_date`          | Date           | End of the plan week                             |
| `meals`             | Array[MealSlot]| Array of meal assignments                        |

---

## 🍽️ MealSlot

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `day`               | Enum           | e.g. "Monday"                                    |
| `time`              | Enum           | e.g. "Breakfast", "Lunch", "Dinner"              |
| `recipe_id`         | UUID           | Foreign key to `Recipe`                          |
| `participants`      | Array[UUID]    | User IDs sharing this meal                       |

---

## 📖 Recipe

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `id`                | UUID           | Unique recipe ID                                 |
| `title`             | String         | Name of the dish                                 |
| `ingredients`       | Array[IngredientItem] | List of ingredients                          |
| `instructions`      | Text           | Cooking steps                                    |
| `tags`              | Array[String]  | e.g. ["vegetarian", "low-carb", "easy"]          |
| `estimated_time_minutes` | Number    | Total prep + cook time                           |
| `storage_type`      | Enum           | fridge, freezer, pantry                          |

---

## 🧂 IngredientItem

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `name`              | String         | e.g. "Carrot", "Chicken Breast"                  |
| `quantity`          | Float          | e.g. 2.5                                         |
| `unit`              | String         | e.g. "cups", "grams", "pieces"                   |
| `storage_type`      | Enum           | fridge, freezer, pantry                          |
| `recipe_id`         | UUID           | Belongs to a specific recipe                     |

---

## 🛒 ShoppingList

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `id`                | UUID           | Unique shopping list ID                          |
| `meal_plan_id`      | UUID           | Reference to MealPlan                            |
| `user_id`           | UUID           | Creator of the list                              |
| `items`             | Array[ShoppingItem] | List of ingredients needed                    |
| `assigned_to`       | Map<UUID, [String]> | Optional: Assign ingredients to users        |
| `created_at`        | Timestamp      | When list was generated                          |

---

## 🛍️ ShoppingItem

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `name`              | String         | e.g. "Milk"                                      |
| `quantity`          | Float          | Combined quantity needed                         |
| `unit`              | String         | e.g. "liters"                                    |
| `storage_type`      | Enum           | fridge, pantry, freezer                          |
| `category`          | String         | e.g. Produce, Dairy, Meat                        |
| `is_purchased`      | Boolean        | Checkbox for completion                          |

---

## 🧾 Inventory (Optional MVP+)

| Field                | Type           | Description                                      |
|---------------------|----------------|--------------------------------------------------|
| `id`                | UUID           | Unique item ID                                   |
| `user_id`           | UUID           | Who added it                                     |
| `name`              | String         | e.g. "Spinach"                                   |
| `quantity`          | Float          | Amount available                                 |
| `unit`              | String         | e.g. "grams", "bags"                             |
| `storage_type`      | Enum           | fridge, pantry, freezer                          |
| `added_on`          | Timestamp      | When it was logged                               |
| `expires_on`        | Date           | Optional expiration date                         |

---

## 🔄 Entity Relationships

- `User` ↔ `MealPlan` (1-to-many)
- `MealPlan` ↔ `MealSlot` (1-to-many)
- `MealSlot` → `Recipe` (many-to-1)
- `Recipe` ↔ `IngredientItem` (1-to-many)
- `MealPlan` → `ShoppingList` (1-to-1)
- `ShoppingList` ↔ `ShoppingItem` (1-to-many)
- `User` ↔ `House` (many-to-1 or many-to-many, if needed)
