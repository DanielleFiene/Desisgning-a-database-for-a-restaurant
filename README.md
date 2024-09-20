## Restaurant Database Schema - README

### Introduction
This project sets up a **restaurant database** with multiple tables representing various entities related to restaurants, such as restaurant details, addresses, categories of dishes, and customer reviews. The schema defines relationships between these entities, enabling efficient data storage and retrieval.

### Database Tables
The schema contains six main tables:

1. **restaurant**: Stores information about each restaurant.
2. **address**: Stores the address details for each restaurant.
3. **category**: Defines different food categories (e.g., Chicken, House Specials, Luncheon Specials).
4. **dish**: Stores individual dish details (name, description, and spice level).
5. **categories_dishes**: A cross-reference table that defines the many-to-many relationship between categories and dishes, with pricing information.
6. **review**: Stores customer reviews for each restaurant, including ratings and descriptions.

### Schema Overview

1. **restaurant table**:
   - Stores basic restaurant information: `id`, `name`, `description`, `rating`, `telephone`, and `hours`.
   - Primary key: `id`.

2. **address table**:
   - Contains restaurant address details with columns such as `street_number`, `street_name`, `city`, `state`, and `google_map_link`.
   - A one-to-one relationship with the `restaurant` table via `restaurant_id` (which is `UNIQUE` and a `FOREIGN KEY` referencing the restaurant table).

3. **category table**:
   - Defines food categories with `id`, `name`, and `description` columns.
   - Primary key: `id`.

4. **dish table**:
   - Stores individual dishes with attributes like `name`, `description`, and whether the dish is hot and spicy.
   - Primary key: `id`.

5. **categories_dishes table**:
   - Cross-reference table for the many-to-many relationship between `category` and `dish`.
   - Columns: `category_id`, `dish_id`, `price`.
   - Primary key: Composite key (`category_id`, `dish_id`).

6. **review table**:
   - Stores customer reviews with columns for `rating`, `description`, `date`, and `restaurant_id`.
   - Foreign key: `restaurant_id` references the `restaurant(id)`.

### Learning Goals
- **Understanding Entity-Relationship Modeling**:
   - Learn how to create normalized database schemas with clearly defined relationships (one-to-one, one-to-many, many-to-many).
   - Understand primary and foreign key constraints for enforcing data integrity.
   
- **Primary and Foreign Keys**:
   - Comprehend the use of primary keys to uniquely identify records in each table.
   - Utilize foreign keys to establish relationships between tables (e.g., `restaurant_id` in the `address` table).

- **Composite Keys**:
   - Learn how to use composite keys in the `categories_dishes` table to model many-to-many relationships between categories and dishes.

- **SQL Queries**:
   - Perform basic to advanced SQL queries, including `JOIN` operations, `GROUP BY`, `HAVING`, and `ORDER BY` clauses to retrieve and analyze data.
   - Examples:
     - Querying dishes by category and price.
     - Finding the most highly rated restaurant.
     - Counting how many times a dish appears across different categories.

- **Data Insertion**:
   - Learn how to insert data into multiple tables while maintaining relationships through foreign keys.
   - Ensure data integrity by populating cross-reference tables like `categories_dishes` with appropriate values.

- **Advanced Query Techniques**:
   - Use of `MAX()`, `COUNT()`, `GROUP BY`, and `HAVING` to aggregate data and gain insights (e.g., highest-rated restaurant, dishes that appear in multiple categories).

### Example Queries
1. **Find all restaurants and their address information**:
   ```sql
   SELECT name, street_number, street_name, telephone
   FROM restaurant
   JOIN address ON restaurant.id = address.restaurant_id;
   ```

2. **Get the best rating for each restaurant**:
   ```sql
   SELECT name, MAX(review.rating) AS best_rating
   FROM review
   JOIN restaurant ON review.restaurant_id = restaurant.id
   GROUP BY name;
   ```

3. **List dishes with their categories and prices**:
   ```sql
   SELECT dish.name AS dish_name, categories_dishes.price AS price, category.name AS category
   FROM categories_dishes
   JOIN dish ON categories_dishes.dish_id = dish.id
   JOIN category ON categories_dishes.category_id = category.id
   ORDER BY dish_name ASC;
   ```

4. **Find all spicy dishes and their categories**:
   ```sql
   SELECT dish.name AS spicy_dish_name, category.name AS category, categories_dishes.price AS price
   FROM categories_dishes
   JOIN dish ON categories_dishes.dish_id = dish.id
   JOIN category ON categories_dishes.category_id = category.id
   WHERE hot_and_spicy = true
   ORDER BY spicy_dish_name ASC;
   ```

5. **Count how many times each dish appears in different categories**:
   ```sql
   SELECT dish_id, COUNT(dish_id) AS dish_count
   FROM categories_dishes
   GROUP BY dish_id
   HAVING COUNT(dish_id) > 1;
   ```

### Conclusion
This schema provides a solid foundation for modeling a restaurant’s menu, categories, dishes, reviews, and related information. By studying the relationships between these entities and writing SQL queries, you will gain a deep understanding of database design and querying in relational databases like PostgreSQL.
