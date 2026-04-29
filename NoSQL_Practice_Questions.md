# NoSQL Practice Questions with Answers

## How to Use This Guide
1. Go to [MongoDB Playground](https://mongoplayground.net/) or install MongoDB locally
2. For Redis: Use [Try Redis](https://redis.io/) or install Redis locally
3. Copy the **Setup Code** for each section
4. Try solving the questions yourself first
5. Check your answer against the solution
6. Study the explanation to understand why it works

---

## Section 1: MongoDB - Basic CRUD Operations

### Setup Code
```javascript
// Create users collection with sample data
db.users.insertMany([
    {
        _id: 1,
        name: "Rahul Kumar",
        email: "rahul@example.com",
        age: 25,
        city: "Mumbai",
        skills: ["JavaScript", "Python", "MongoDB"],
        salary: 75000,
        isActive: true,
        joinDate: new Date("2023-01-15")
    },
    {
        _id: 2,
        name: "Priya Sharma",
        email: "priya@example.com",
        age: 24,
        city: "Delhi",
        skills: ["Java", "SQL", "Spring"],
        salary: 65000,
        isActive: true,
        joinDate: new Date("2023-03-20")
    },
    {
        _id: 3,
        name: "Amit Patel",
        email: "amit@example.com",
        age: 28,
        city: "Bangalore",
        skills: ["Python", "Django", "MongoDB", "AWS"],
        salary: 80000,
        isActive: false,
        joinDate: new Date("2022-11-10")
    },
    {
        _id: 4,
        name: "Sneha Reddy",
        email: "sneha@example.com",
        age: 23,
        city: "Hyderabad",
        skills: ["React", "Node.js", "MongoDB"],
        salary: 55000,
        isActive: true,
        joinDate: new Date("2024-02-01")
    },
    {
        _id: 5,
        name: "Ravi Verma",
        email: "ravi@example.com",
        age: 27,
        city: "Mumbai",
        skills: ["Python", "Machine Learning", "TensorFlow"],
        salary: 70000,
        isActive: true,
        joinDate: new Date("2023-06-15")
    }
]);
```

---

### Q1: Find All Users
**Question:** Retrieve all users from the collection.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find();
```

**Explanation:** `.find()` with no parameters returns all documents.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", email: "rahul@example.com", ... },
    { _id: 2, name: "Priya Sharma", email: "priya@example.com", ... },
    // ... all 5 users
]
```
</details>

---

### Q2: Find User by Email
**Question:** Find the user with email "rahul@example.com".

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.findOne({ email: "rahul@example.com" });
```

**Explanation:** `findOne()` returns a single document matching the filter.

**Expected Output:**
```javascript
{
    _id: 1,
    name: "Rahul Kumar",
    email: "rahul@example.com",
    age: 25,
    city: "Mumbai",
    skills: ["JavaScript", "Python", "MongoDB"],
    salary: 75000,
    isActive: true,
    joinDate: ISODate("2023-01-15T00:00:00.000Z")
}
```
</details>

---

### Q3: Find Users by City
**Question:** Find all users who live in Mumbai.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({ city: "Mumbai" });
```

**Explanation:** Filter documents where the `city` field matches "Mumbai".

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", city: "Mumbai", ... },
    { _id: 5, name: "Ravi Verma", city: "Mumbai", ... }
]
```
</details>

---

### Q4: Find Users with Salary Greater Than 65000
**Question:** Find all users earning more than 65,000.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({ salary: { $gt: 65000 } });
```

**Explanation:** `$gt` (greater than) is a comparison operator.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", salary: 75000, ... },
    { _id: 3, name: "Amit Patel", salary: 80000, ... },
    { _id: 5, name: "Ravi Verma", salary: 70000, ... }
]
```
</details>

---

### Q5: Find Active Users in Mumbai
**Question:** Find all active users who live in Mumbai.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({ 
    city: "Mumbai", 
    isActive: true 
});
```

**Explanation:** Multiple conditions in the filter object work as AND.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", city: "Mumbai", isActive: true, ... },
    { _id: 5, name: "Ravi Verma", city: "Mumbai", isActive: true, ... }
]
```
</details>

---

### Q6: Find Users from Mumbai OR Delhi
**Question:** Find users who live in either Mumbai or Delhi.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
// Method 1: Using $in
db.users.find({ city: { $in: ["Mumbai", "Delhi"] } });

// Method 2: Using $or
db.users.find({
    $or: [
        { city: "Mumbai" },
        { city: "Delhi" }
    ]
});
```

**Explanation:** `$in` checks if field value is in the array. `$or` accepts an array of conditions.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", city: "Mumbai", ... },
    { _id: 2, name: "Priya Sharma", city: "Delhi", ... },
    { _id: 5, name: "Ravi Verma", city: "Mumbai", ... }
]
```
</details>

---

## Section 2: MongoDB - Projection and Sorting

### Setup Code
Use the same users collection from Section 1.

---

### Q7: Get Only Names and Emails
**Question:** Retrieve only the name and email fields for all users (exclude _id).

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({}, { name: 1, email: 1, _id: 0 });
```

**Explanation:** Projection object: `1` includes field, `0` excludes. `_id` is included by default, so explicitly exclude it.

**Expected Output:**
```javascript
[
    { name: "Rahul Kumar", email: "rahul@example.com" },
    { name: "Priya Sharma", email: "priya@example.com" },
    { name: "Amit Patel", email: "amit@example.com" },
    { name: "Sneha Reddy", email: "sneha@example.com" },
    { name: "Ravi Verma", email: "ravi@example.com" }
]
```
</details>

---

### Q8: Sort Users by Salary (Highest First)
**Question:** Get all users sorted by salary in descending order.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find().sort({ salary: -1 });
```

**Explanation:** `.sort()` with `-1` sorts descending (highest first), `1` sorts ascending.

**Expected Output:**
```javascript
[
    { _id: 3, name: "Amit Patel", salary: 80000, ... },
    { _id: 1, name: "Rahul Kumar", salary: 75000, ... },
    { _id: 5, name: "Ravi Verma", salary: 70000, ... },
    { _id: 2, name: "Priya Sharma", salary: 65000, ... },
    { _id: 4, name: "Sneha Reddy", salary: 55000, ... }
]
```
</details>

---

### Q9: Get Top 3 Youngest Users
**Question:** Find the 3 youngest users.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find().sort({ age: 1 }).limit(3);
```

**Explanation:** Sort by age ascending (youngest first), then limit to 3 results.

**Expected Output:**
```javascript
[
    { _id: 4, name: "Sneha Reddy", age: 23, ... },
    { _id: 2, name: "Priya Sharma", age: 24, ... },
    { _id: 1, name: "Rahul Kumar", age: 25, ... }
]
```
</details>

---

### Q10: Pagination - Get Users 3-4
**Question:** Skip the first 2 users and get the next 2 (pagination).

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find().skip(2).limit(2);
```

**Explanation:** `.skip(2)` skips first 2 documents, `.limit(2)` returns only 2.

**Expected Output:**
```javascript
[
    { _id: 3, name: "Amit Patel", ... },
    { _id: 4, name: "Sneha Reddy", ... }
]
```
</details>

---

## Section 3: MongoDB - Array Queries

### Setup Code
Use the same users collection from Section 1.

---

### Q11: Find Users with Python Skill
**Question:** Find all users who have "Python" in their skills array.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({ skills: "Python" });
```

**Explanation:** MongoDB automatically searches inside arrays for matching values.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", skills: ["JavaScript", "Python", "MongoDB"], ... },
    { _id: 3, name: "Amit Patel", skills: ["Python", "Django", "MongoDB", "AWS"], ... },
    { _id: 5, name: "Ravi Verma", skills: ["Python", "Machine Learning", "TensorFlow"], ... }
]
```
</details>

---

### Q12: Find Users with Multiple Skills
**Question:** Find users who have BOTH "Python" AND "MongoDB" skills.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({ skills: { $all: ["Python", "MongoDB"] } });
```

**Explanation:** `$all` matches documents where the array contains all specified values.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", skills: ["JavaScript", "Python", "MongoDB"], ... },
    { _id: 3, name: "Amit Patel", skills: ["Python", "Django", "MongoDB", "AWS"], ... }
]
```
</details>

---

### Q13: Count Skills
**Question:** Count how many users have exactly 3 skills.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.countDocuments({ skills: { $size: 3 } });
```

**Explanation:** `$size` matches arrays with the exact number of elements.

**Expected Output:**
```javascript
3
// Users with 3 skills: Rahul (3), Priya (3), Sneha (3)
```
</details>

---

## Section 4: MongoDB - Update Operations

### Setup Code
Use the same users collection from Section 1.

---

### Q14: Update User Age
**Question:** Update Rahul Kumar's age to 26.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.updateOne(
    { name: "Rahul Kumar" },
    { $set: { age: 26 } }
);
```

**Explanation:** `updateOne()` updates the first matching document. `$set` modifies specified fields.

**Expected Output:**
```javascript
{
    acknowledged: true,
    matchedCount: 1,
    modifiedCount: 1
}
```
</details>

---

### Q15: Increment All Salaries by 5000
**Question:** Give all active users a 5,000 salary increase.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.updateMany(
    { isActive: true },
    { $inc: { salary: 5000 } }
);
```

**Explanation:** `updateMany()` updates all matching documents. `$inc` increments numeric values.

**Expected Output:**
```javascript
{
    acknowledged: true,
    matchedCount: 4,
    modifiedCount: 4
}
```
</details>

---

### Q16: Add Skill to User
**Question:** Add "Docker" skill to Priya Sharma's skills array.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.updateOne(
    { name: "Priya Sharma" },
    { $push: { skills: "Docker" } }
);
```

**Explanation:** `$push` adds an element to an array.

**Expected Output:**
```javascript
{
    acknowledged: true,
    matchedCount: 1,
    modifiedCount: 1
}
```
</details>

---

### Q17: Remove Skill from User
**Question:** Remove "SQL" from Priya Sharma's skills.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.updateOne(
    { name: "Priya Sharma" },
    { $pull: { skills: "SQL" } }
);
```

**Explanation:** `$pull` removes elements matching the value from an array.

**Expected Output:**
```javascript
{
    acknowledged: true,
    matchedCount: 1,
    modifiedCount: 1
}
```
</details>

---

### Q18: Upsert Operation
**Question:** Update Neha Gupta's age to 22, or insert a new document if she doesn't exist.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.updateOne(
    { name: "Neha Gupta" },
    { $set: { name: "Neha Gupta", age: 22, email: "neha@example.com" } },
    { upsert: true }
);
```

**Explanation:** `upsert: true` creates a new document if no match is found.

**Expected Output:**
```javascript
{
    acknowledged: true,
    matchedCount: 0,
    modifiedCount: 0,
    upsertedId: 6  // New document created
}
```
</details>

---

## Section 5: MongoDB - Aggregation Framework

### Setup Code
```javascript
db.orders.insertMany([
    { _id: 1, customerId: 1, product: "Laptop", quantity: 1, price: 50000, date: new Date("2023-06-15") },
    { _id: 2, customerId: 2, product: "Mouse", quantity: 2, price: 500, date: new Date("2023-06-16") },
    { _id: 3, customerId: 1, product: "Keyboard", quantity: 1, price: 2000, date: new Date("2023-06-17") },
    { _id: 4, customerId: 3, product: "Monitor", quantity: 1, price: 15000, date: new Date("2023-06-18") },
    { _id: 5, customerId: 2, product: "Headphones", quantity: 1, price: 3000, date: new Date("2023-06-19") },
    { _id: 6, customerId: 1, product: "Mouse", quantity: 1, price: 500, date: new Date("2023-06-20") }
]);
```

---

### Q19: Count Total Orders
**Question:** How many total orders are there?

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.orders.countDocuments();

// Or using aggregation
db.orders.aggregate([
    { $count: "totalOrders" }
]);
```

**Explanation:** `countDocuments()` is simpler, but aggregation shows it in a pipeline context.

**Expected Output:**
```javascript
6
// Or: [{ totalOrders: 6 }]
```
</details>

---

### Q20: Calculate Total Revenue
**Question:** What is the total revenue from all orders?

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.orders.aggregate([
    {
        $group: {
            _id: null,
            totalRevenue: { $sum: "$price" }
        }
    }
]);
```

**Explanation:** `$group` with `_id: null` groups all documents together. `$sum` adds up values.

**Expected Output:**
```javascript
[
    { _id: null, totalRevenue: 71000 }
]
```
</details>

---

### Q21: Orders Per Customer
**Question:** How many orders has each customer placed, and what's their total spending?

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.orders.aggregate([
    {
        $group: {
            _id: "$customerId",
            orderCount: { $sum: 1 },
            totalSpent: { $sum: "$price" }
        }
    },
    { $sort: { totalSpent: -1 } }
]);
```

**Explanation:** Group by `customerId`, count orders with `$sum: 1`, and sum prices.

**Expected Output:**
```javascript
[
    { _id: 1, orderCount: 3, totalSpent: 52500 },
    { _id: 3, orderCount: 1, totalSpent: 15000 },
    { _id: 2, orderCount: 2, totalSpent: 3500 }
]
```
</details>

---

### Q22: Average Order Value
**Question:** What is the average order value?

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.orders.aggregate([
    {
        $group: {
            _id: null,
            avgOrderValue: { $avg: "$price" }
        }
    }
]);
```

**Explanation:** `$avg` calculates the average of numeric values.

**Expected Output:**
```javascript
[
    { _id: null, avgOrderValue: 11833.33 }
]
```
</details>

---

### Q23: Most Popular Products
**Question:** Which products have been ordered most frequently?

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.orders.aggregate([
    {
        $group: {
            _id: "$product",
            orderCount: { $sum: 1 },
            totalQuantity: { $sum: "$quantity" }
        }
    },
    { $sort: { orderCount: -1 } }
]);
```

**Explanation:** Group by product, count orders, sum quantities, and sort by popularity.

**Expected Output:**
```javascript
[
    { _id: "Mouse", orderCount: 2, totalQuantity: 3 },
    { _id: "Laptop", orderCount: 1, totalQuantity: 1 },
    { _id: "Keyboard", orderCount: 1, totalQuantity: 1 },
    { _id: "Monitor", orderCount: 1, totalQuantity: 1 },
    { _id: "Headphones", orderCount: 1, totalQuantity: 1 }
]
```
</details>

---

### Q24: Filter and Aggregate
**Question:** Find total revenue from orders above 5,000.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.orders.aggregate([
    { $match: { price: { $gt: 5000 } } },
    {
        $group: {
            _id: null,
            totalRevenue: { $sum: "$price" }
        }
    }
]);
```

**Explanation:** `$match` filters documents before grouping (like WHERE in SQL).

**Expected Output:**
```javascript
[
    { _id: null, totalRevenue: 65000 }
]
```
</details>

---

## Section 6: Redis Practice

### Setup Code
```bash
# Run these commands in Redis CLI or Try Redis online
# No initial setup needed - Redis starts empty
```

---

### Q25: Set and Get User Name
**Question:** Store a user's name and retrieve it.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
SET user:1:name "Rahul Kumar"
GET user:1:name
```

**Explanation:** `SET` stores a key-value pair, `GET` retrieves it.

**Expected Output:**
```
"Rahul Kumar"
```
</details>

---

### Q26: Set with Expiration
**Question:** Store a session token that expires in 60 seconds.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
SETEX session:abc123 60 "user_token_data"
TTL session:abc123
```

**Explanation:** `SETEX key seconds value` sets a key with expiration. `TTL` shows time-to-live.

**Expected Output:**
```
OK
60  # (decreases over time)
```
</details>

---

### Q27: Counter Operations
**Question:** Create a page view counter and increment it 5 times.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
SET page:views 0
INCR page:views
INCR page:views
INCR page:views
INCRBY page:views 2
GET page:views
```

**Explanation:** `INCR` increments by 1, `INCRBY` increments by specified amount.

**Expected Output:**
```
"5"
```
</details>

---

### Q28: Working with Lists
**Question:** Create a task queue with FIFO (First In, First Out) behavior.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
# Add tasks
RPUSH tasks "Send email"
RPUSH tasks "Process payment"
RPUSH tasks "Update inventory"

# View all tasks
LRANGE tasks 0 -1

# Process tasks (remove from left)
LPOP tasks
LPOP tasks
```

**Explanation:** `RPUSH` adds to right (end), `LPOP` removes from left (beginning) - FIFO queue.

**Expected Output:**
```
# LRANGE output:
1) "Send email"
2) "Process payment"
3) "Update inventory"

# LPOP outputs:
"Send email"
"Process payment"
```
</details>

---

### Q29: Working with Sets
**Question:** Track unique visitors to a website.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
# Add visitors (duplicates automatically ignored)
SADD visitors:today "user1"
SADD visitors:today "user2"
SADD visitors:today "user1"  # Duplicate - won't be added
SADD visitors:today "user3"

# Count unique visitors
SCARD visitors:today

# Check if user visited
SISMEMBER visitors:today "user2"
```

**Explanation:** Sets store unique values. `SCARD` counts members, `SISMEMBER` checks membership.

**Expected Output:**
```
3  # Only 3 unique visitors
1  # Yes, user2 is a member
```
</details>

---

### Q30: Working with Hashes
**Question:** Store user profile information.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
# Store user data
HSET user:100 name "Rahul Kumar"
HSET user:100 email "rahul@example.com"
HSET user:100 age 25

# Or set multiple at once
HMSET user:101 name "Priya Sharma" email "priya@example.com" age 24

# Get specific field
HGET user:100 name

# Get all fields
HGETALL user:100

# Increment age
HINCRBY user:100 age 1
```

**Explanation:** Hashes store field-value pairs, perfect for objects. `HGETALL` retrieves all fields.

**Expected Output:**
```
# HGET output:
"Rahul Kumar"

# HGETALL output:
1) "name"
2) "Rahul Kumar"
3) "email"
4) "rahul@example.com"
5) "age"
6) "26"  # After increment
```
</details>

---

### Q31: Leaderboard with Sorted Sets
**Question:** Create a game leaderboard and track top players.

<details>
<summary>Click to see answer</summary>

**Solution:**
```bash
# Add players with scores
ZADD leaderboard 1500 "Rahul"
ZADD leaderboard 2000 "Priya"
ZADD leaderboard 1800 "Amit"
ZADD leaderboard 1200 "Sneha"

# Get top 3 players
ZREVRANGE leaderboard 0 2 WITHSCORES

# Get Rahul's rank (0-based)
ZREVRANK leaderboard "Rahul"

# Increment Rahul's score by 500
ZINCRBY leaderboard 500 "Rahul"

# Get players with scores between 1500-2000
ZRANGEBYSCORE leaderboard 1500 2000
```

**Explanation:** Sorted sets maintain order by score. `ZREVRANGE` gets highest scores first.

**Expected Output:**
```
# Top 3:
1) "Priya"
2) "2000"
3) "Rahul"
4) "2000"  # After increment
5) "Amit"
6) "1800"

# Rahul's rank:
1  # Second place (0-indexed)
```
</details>

---

## Section 7: Advanced MongoDB Queries

### Q32: Text Search
**Question:** Find users whose names contain "Kumar" (case-insensitive).

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
// Using regex
db.users.find({ name: /kumar/i });

// Or more explicit
db.users.find({ name: { $regex: "kumar", $options: "i" } });
```

**Explanation:** Regex with `i` flag makes search case-insensitive.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", ... }
]
```
</details>

---

### Q33: Date Range Query
**Question:** Find users who joined in 2023.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.find({
    joinDate: {
        $gte: new Date("2023-01-01"),
        $lt: new Date("2024-01-01")
    }
});
```

**Explanation:** `$gte` (greater than or equal) and `$lt` (less than) define date range.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", joinDate: ISODate("2023-01-15"), ... },
    { _id: 2, name: "Priya Sharma", joinDate: ISODate("2023-03-20"), ... },
    { _id: 5, name: "Ravi Verma", joinDate: ISODate("2023-06-15"), ... }
]
```
</details>

---

### Q34: Complex Aggregation Pipeline
**Question:** Find the average salary by city, but only for cities with more than 1 user.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.aggregate([
    {
        $group: {
            _id: "$city",
            userCount: { $sum: 1 },
            avgSalary: { $avg: "$salary" }
        }
    },
    {
        $match: { userCount: { $gt: 1 } }
    },
    {
        $sort: { avgSalary: -1 }
    }
]);
```

**Explanation:** Group by city, calculate average, filter groups with HAVING-like `$match`, then sort.

**Expected Output:**
```javascript
[
    { _id: "Mumbai", userCount: 2, avgSalary: 72500 }
]
```
</details>

---

## 🎯 Challenge Questions

### Q35: Complex Join Operation
**Question:** Join users with their orders and show each user's name with their total spending.

<details>
<summary>Click to see answer</summary>

**Solution:**
```javascript
db.users.aggregate([
    {
        $lookup: {
            from: "orders",
            localField: "_id",
            foreignField: "customerId",
            as: "userOrders"
        }
    },
    {
        $project: {
            name: 1,
            totalSpent: { $sum: "$userOrders.price" },
            orderCount: { $size: "$userOrders" }
        }
    },
    {
        $sort: { totalSpent: -1 }
    }
]);
```

**Explanation:** `$lookup` performs a join, `$project` calculates totals, `$size` counts array elements.

**Expected Output:**
```javascript
[
    { _id: 1, name: "Rahul Kumar", totalSpent: 52500, orderCount: 3 },
    { _id: 3, name: "Amit Patel", totalSpent: 15000, orderCount: 1 },
    { _id: 2, name: "Priya Sharma", totalSpent: 3500, orderCount: 2 },
    { _id: 4, name: "Sneha Reddy", totalSpent: 0, orderCount: 0 },
    { _id: 5, name: "Ravi Verma", totalSpent: 0, orderCount: 0 }
]
```
</details>

---

## 📚 Practice Tips

1. **Experiment**: Try modifying queries to see different results
2. **Read Errors**: MongoDB error messages are helpful - read them carefully
3. **Use Explain**: Add `.explain()` to understand query performance
4. **Check Documentation**: MongoDB docs are excellent - refer to them often
5. **Practice Daily**: Try to solve at least 2-3 queries daily

---

## 🔗 Additional Resources

- **MongoDB University**: Free comprehensive courses
- **Redis University**: Interactive Redis tutorials
- **Practice Platforms**:
  - MongoDB Atlas (free tier with sample data)
  - Leetcode (has some NoSQL questions)
  - HackerRank
  
---

## 🚀 Next Steps

After mastering these basics:
1. Learn about **indexes** and query optimization
2. Understand **replication** and **sharding**
3. Explore **transactions** in MongoDB
4. Study **data modeling** patterns
5. Practice with real-world projects

Happy Learning! 🎓
