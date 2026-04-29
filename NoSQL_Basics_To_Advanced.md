# NoSQL Basics To Advanced

## 1. What is NoSQL?
NoSQL (Not Only SQL) databases are non-relational database management systems designed for:
- Flexible schemas (dynamic data structures)
- Horizontal scalability (distributed across multiple servers)
- High performance for specific use cases
- Handling unstructured or semi-structured data

---

## 2. Types of NoSQL Databases

### Document Stores
Store data as JSON-like documents
- **Examples**: MongoDB, CouchDB, Firestore
- **Best for**: Content management, catalogs, user profiles

### Key-Value Stores
Simple key-value pairs for fast lookups
- **Examples**: Redis, DynamoDB, Memcached
- **Best for**: Caching, session management, shopping carts

### Column-Family Stores
Store data in columns rather than rows
- **Examples**: Cassandra, HBase, ScyllaDB
- **Best for**: Time-series data, analytics, IoT

### Graph Databases
Store data as nodes and relationships
- **Examples**: Neo4j, Amazon Neptune, ArangoDB
- **Best for**: Social networks, recommendation engines, fraud detection

---

## 3. MongoDB Basics (Document Store)

MongoDB is the most popular NoSQL database. Documents are stored in BSON (Binary JSON) format.

### Basic Terminology
| SQL Term | MongoDB Term |
|----------|--------------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | _id (automatic) |

### Connecting to MongoDB
```javascript
// Using mongosh (MongoDB Shell)
mongosh "mongodb://localhost:27017"

// Show databases
show dbs

// Create/switch to database
use myDatabase

// Show collections
show collections
```

---

## 4. CRUD Operations (MongoDB)

### Create (Insert)

#### Insert One Document
```javascript
db.users.insertOne({
    name: "Rahul Kumar",
    email: "rahul@example.com",
    age: 25,
    city: "Mumbai",
    skills: ["JavaScript", "Python", "MongoDB"],
    isActive: true
});
```

#### Insert Multiple Documents
```javascript
db.users.insertMany([
    {
        name: "Priya Sharma",
        email: "priya@example.com",
        age: 24,
        city: "Delhi",
        skills: ["Java", "SQL"],
        isActive: true
    },
    {
        name: "Amit Patel",
        email: "amit@example.com",
        age: 28,
        city: "Bangalore",
        skills: ["Python", "Django", "MongoDB"],
        isActive: false
    }
]);
```

### Read (Query)

#### Find All Documents
```javascript
db.users.find();

// Pretty print
db.users.find().pretty();
```

#### Find with Filter
```javascript
// Find by exact match
db.users.find({ city: "Mumbai" });

// Find by age greater than 25
db.users.find({ age: { $gt: 25 } });

// Find active users
db.users.find({ isActive: true });
```

#### Find One Document
```javascript
db.users.findOne({ email: "rahul@example.com" });
```

#### Query Operators
```javascript
// Comparison Operators
db.users.find({ age: { $eq: 25 } });      // Equal to
db.users.find({ age: { $ne: 25 } });      // Not equal
db.users.find({ age: { $gt: 25 } });      // Greater than
db.users.find({ age: { $gte: 25 } });     // Greater than or equal
db.users.find({ age: { $lt: 30 } });      // Less than
db.users.find({ age: { $lte: 30 } });     // Less than or equal

// IN operator
db.users.find({ city: { $in: ["Mumbai", "Delhi"] } });

// NOT IN operator
db.users.find({ city: { $nin: ["Mumbai", "Delhi"] } });
```

#### Logical Operators
```javascript
// AND (implicit)
db.users.find({ city: "Mumbai", age: { $gt: 20 } });

// OR
db.users.find({
    $or: [
        { city: "Mumbai" },
        { city: "Delhi" }
    ]
});

// AND with OR
db.users.find({
    $and: [
        { age: { $gt: 20 } },
        { $or: [{ city: "Mumbai" }, { city: "Delhi" }] }
    ]
});

// NOT
db.users.find({ age: { $not: { $gt: 25 } } });
```

#### Array Queries
```javascript
// Find users with JavaScript skill
db.users.find({ skills: "JavaScript" });

// Find users with ALL specified skills
db.users.find({ skills: { $all: ["JavaScript", "Python"] } });

// Find documents where array size is 3
db.users.find({ skills: { $size: 3 } });

// Find using $elemMatch
db.orders.find({
    items: {
        $elemMatch: { product: "Laptop", quantity: { $gte: 2 } }
    }
});
```

#### Projection (Select Specific Fields)
```javascript
// Include only name and email (exclude _id)
db.users.find({}, { name: 1, email: 1, _id: 0 });

// Exclude specific fields
db.users.find({}, { password: 0, ssn: 0 });
```

### Update

#### Update One Document
```javascript
// Set new values
db.users.updateOne(
    { email: "rahul@example.com" },
    { $set: { age: 26, city: "Pune" } }
);

// Increment a value
db.users.updateOne(
    { email: "rahul@example.com" },
    { $inc: { age: 1 } }
);

// Add to array
db.users.updateOne(
    { email: "rahul@example.com" },
    { $push: { skills: "React" } }
);

// Remove from array
db.users.updateOne(
    { email: "rahul@example.com" },
    { $pull: { skills: "React" } }
);
```

#### Update Multiple Documents
```javascript
db.users.updateMany(
    { city: "Mumbai" },
    { $set: { country: "India" } }
);
```

#### Replace Entire Document
```javascript
db.users.replaceOne(
    { email: "rahul@example.com" },
    {
        name: "Rahul Kumar",
        email: "rahul@example.com",
        age: 26,
        city: "Pune"
    }
);
```

#### Upsert (Update or Insert)
```javascript
db.users.updateOne(
    { email: "neha@example.com" },
    { $set: { name: "Neha Gupta", age: 23 } },
    { upsert: true }  // Insert if not found
);
```

### Delete

#### Delete One Document
```javascript
db.users.deleteOne({ email: "rahul@example.com" });
```

#### Delete Multiple Documents
```javascript
db.users.deleteMany({ isActive: false });

// Delete all documents in collection
db.users.deleteMany({});
```

---

## 5. Sorting and Limiting

### Sort
```javascript
// Sort by age ascending (1)
db.users.find().sort({ age: 1 });

// Sort by age descending (-1)
db.users.find().sort({ age: -1 });

// Sort by multiple fields
db.users.find().sort({ city: 1, age: -1 });
```

### Limit and Skip
```javascript
// Get first 5 users
db.users.find().limit(5);

// Skip first 10, get next 5 (pagination)
db.users.find().skip(10).limit(5);

// Combine: sort, skip, limit
db.users.find()
    .sort({ age: -1 })
    .skip(0)
    .limit(10);
```

### Count
```javascript
// Count all documents
db.users.countDocuments();

// Count with filter
db.users.countDocuments({ city: "Mumbai" });

// Estimated count (faster, less accurate)
db.users.estimatedDocumentCount();
```

---

## 6. Aggregation Framework

Aggregation pipelines process documents through multiple stages.

### Basic Aggregation
```javascript
// Group by city and count
db.users.aggregate([
    {
        $group: {
            _id: "$city",
            count: { $sum: 1 },
            avgAge: { $avg: "$age" }
        }
    }
]);
```

### Common Pipeline Stages

#### $match (Filter)
```javascript
db.users.aggregate([
    { $match: { age: { $gte: 25 } } }
]);
```

#### $group (Group and Aggregate)
```javascript
db.orders.aggregate([
    {
        $group: {
            _id: "$customerId",
            totalSpent: { $sum: "$amount" },
            orderCount: { $sum: 1 },
            avgOrder: { $avg: "$amount" },
            maxOrder: { $max: "$amount" },
            minOrder: { $min: "$amount" }
        }
    }
]);
```

#### $project (Select/Transform Fields)
```javascript
db.users.aggregate([
    {
        $project: {
            name: 1,
            email: 1,
            ageCategory: {
                $cond: {
                    if: { $gte: ["$age", 25] },
                    then: "Adult",
                    else: "Young"
                }
            }
        }
    }
]);
```

#### $sort
```javascript
db.users.aggregate([
    { $sort: { age: -1 } }
]);
```

#### $limit and $skip
```javascript
db.users.aggregate([
    { $sort: { age: -1 } },
    { $skip: 10 },
    { $limit: 5 }
]);
```

#### $unwind (Flatten Arrays)
```javascript
// Convert array elements to separate documents
db.users.aggregate([
    { $unwind: "$skills" },
    {
        $group: {
            _id: "$skills",
            userCount: { $sum: 1 }
        }
    },
    { $sort: { userCount: -1 } }
]);
```

#### $lookup (Join)
```javascript
// Join users with orders
db.users.aggregate([
    {
        $lookup: {
            from: "orders",
            localField: "_id",
            foreignField: "userId",
            as: "userOrders"
        }
    }
]);
```

### Complete Aggregation Example
```javascript
db.orders.aggregate([
    // Stage 1: Filter orders from 2023
    {
        $match: {
            orderDate: {
                $gte: ISODate("2023-01-01"),
                $lt: ISODate("2024-01-01")
            }
        }
    },
    // Stage 2: Join with customers
    {
        $lookup: {
            from: "customers",
            localField: "customerId",
            foreignField: "_id",
            as: "customer"
        }
    },
    // Stage 3: Unwind customer array
    { $unwind: "$customer" },
    // Stage 4: Group by customer city
    {
        $group: {
            _id: "$customer.city",
            totalRevenue: { $sum: "$amount" },
            orderCount: { $sum: 1 },
            avgOrderValue: { $avg: "$amount" }
        }
    },
    // Stage 5: Sort by revenue
    { $sort: { totalRevenue: -1 } },
    // Stage 6: Limit to top 10 cities
    { $limit: 10 }
]);
```

---

## 7. Indexes

Indexes improve query performance on large collections.

### Create Index
```javascript
// Single field index
db.users.createIndex({ email: 1 });  // 1 = ascending, -1 = descending

// Compound index
db.users.createIndex({ city: 1, age: -1 });

// Unique index
db.users.createIndex({ email: 1 }, { unique: true });

// Text index (for text search)
db.articles.createIndex({ title: "text", content: "text" });
```

### View Indexes
```javascript
db.users.getIndexes();
```

### Drop Index
```javascript
db.users.dropIndex("email_1");

// Drop all indexes except _id
db.users.dropIndexes();
```

### Explain Query Performance
```javascript
db.users.find({ email: "rahul@example.com" }).explain("executionStats");
```

---

## 8. Schema Design Patterns

### Embedding (Nested Documents)
Best for one-to-few relationships:

```javascript
// User with embedded address
{
    _id: ObjectId("..."),
    name: "Rahul Kumar",
    email: "rahul@example.com",
    address: {
        street: "MG Road",
        city: "Mumbai",
        state: "Maharashtra",
        pincode: "400001"
    },
    orders: [
        { orderId: 101, product: "Laptop", amount: 50000 },
        { orderId: 102, product: "Mouse", amount: 500 }
    ]
}
```

**Pros:**
- Fast reads (single query)
- Data locality
- Atomic updates

**Cons:**
- Document size limit (16MB)
- Data duplication
- Not suitable for large arrays

### Referencing (Normalized)
Best for one-to-many or many-to-many:

```javascript
// Users collection
{
    _id: ObjectId("userId1"),
    name: "Rahul Kumar",
    email: "rahul@example.com"
}

// Orders collection
{
    _id: ObjectId("orderId1"),
    userId: ObjectId("userId1"),  // Reference
    product: "Laptop",
    amount: 50000,
    date: ISODate("2023-06-15")
}
```

**Pros:**
- No duplication
- Smaller documents
- Flexible relationships

**Cons:**
- Multiple queries or $lookup
- More complex code

### Hybrid Approach
Combine both for optimal performance:

```javascript
{
    _id: ObjectId("orderId1"),
    userId: ObjectId("userId1"),
    userName: "Rahul Kumar",  // Denormalized for quick access
    product: "Laptop",
    amount: 50000
}
```

---

## 9. Redis Basics (Key-Value Store)

Redis is an in-memory data store, extremely fast for caching and real-time operations.

### Basic Data Types

#### Strings
```bash
# Set a key
SET user:1:name "Rahul Kumar"

# Get a key
GET user:1:name

# Set with expiration (10 seconds)
SETEX session:abc123 10 "user_data"

# Set if not exists
SETNX user:1:email "rahul@example.com"

# Increment
SET counter 100
INCR counter  # 101
INCRBY counter 10  # 111

# Multiple set/get
MSET user:1:name "Rahul" user:1:age "25"
MGET user:1:name user:1:age
```

#### Lists (Ordered)
```bash
# Push to list
LPUSH users "Rahul"  # Add to left
RPUSH users "Priya"  # Add to right

# Get list elements
LRANGE users 0 -1  # Get all
LRANGE users 0 10  # First 10

# Pop from list
LPOP users  # Remove from left
RPOP users  # Remove from right

# List length
LLEN users
```

#### Sets (Unique, Unordered)
```bash
# Add to set
SADD skills:user1 "JavaScript"
SADD skills:user1 "Python"
SADD skills:user1 "MongoDB"

# Get all members
SMEMBERS skills:user1

# Check membership
SISMEMBER skills:user1 "Python"  # 1 (true)

# Set operations
SADD skills:user2 "Python" "Java" "SQL"
SINTER skills:user1 skills:user2  # Intersection
SUNION skills:user1 skills:user2  # Union
SDIFF skills:user1 skills:user2   # Difference

# Remove from set
SREM skills:user1 "JavaScript"
```

#### Hashes (Field-Value Pairs)
```bash
# Set hash fields
HSET user:1 name "Rahul Kumar"
HSET user:1 email "rahul@example.com"
HSET user:1 age 25

# Set multiple fields
HMSET user:2 name "Priya" email "priya@example.com" age 24

# Get field value
HGET user:1 name

# Get all fields
HGETALL user:1

# Get multiple fields
HMGET user:1 name email

# Increment field
HINCRBY user:1 age 1
```

#### Sorted Sets (Ordered by Score)
```bash
# Add with score
ZADD leaderboard 1000 "Rahul"
ZADD leaderboard 1500 "Priya"
ZADD leaderboard 800 "Amit"

# Get range by rank
ZRANGE leaderboard 0 -1  # Lowest to highest
ZREVRANGE leaderboard 0 -1  # Highest to lowest

# Get range with scores
ZRANGE leaderboard 0 -1 WITHSCORES

# Get rank
ZRANK leaderboard "Rahul"

# Increment score
ZINCRBY leaderboard 100 "Rahul"

# Get by score range
ZRANGEBYSCORE leaderboard 900 1200
```

### Expiration and TTL
```bash
# Set expiration
EXPIRE user:session:abc 3600  # Expire in 3600 seconds

# Check TTL
TTL user:session:abc

# Remove expiration
PERSIST user:session:abc
```

---

## 10. Cassandra Basics (Column-Family)

Cassandra is designed for high availability and scalability.

### CQL (Cassandra Query Language)
```sql
-- Create keyspace (like database)
CREATE KEYSPACE myapp
WITH replication = {
    'class': 'SimpleStrategy',
    'replication_factor': 3
};

-- Use keyspace
USE myapp;

-- Create table
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    name TEXT,
    email TEXT,
    age INT,
    city TEXT,
    created_at TIMESTAMP
);

-- Insert data
INSERT INTO users (user_id, name, email, age, city, created_at)
VALUES (uuid(), 'Rahul Kumar', 'rahul@example.com', 25, 'Mumbai', toTimestamp(now()));

-- Query data
SELECT * FROM users WHERE user_id = 550e8400-e29b-41d4-a716-446655440000;

-- Update data
UPDATE users SET age = 26 WHERE user_id = 550e8400-e29b-41d4-a716-446655440000;

-- Delete data
DELETE FROM users WHERE user_id = 550e8400-e29b-41d4-a716-446655440000;
```

### Important Concepts
- **Partition Key**: Determines data distribution
- **Clustering Key**: Determines sorting within partition
- **No JOINs**: Denormalize data for read performance
- **Write Optimized**: Very fast writes, eventual consistency

---

## 11. Common NoSQL Patterns

### Caching Pattern (Redis)
```javascript
// Pseudo-code
function getUserData(userId) {
    // Try cache first
    const cached = redis.get(`user:${userId}`);
    if (cached) return JSON.parse(cached);
    
    // Cache miss - query database
    const user = db.users.findOne({ _id: userId });
    
    // Store in cache for 1 hour
    redis.setex(`user:${userId}`, 3600, JSON.stringify(user));
    
    return user;
}
```

### Time-Series Data (Cassandra/MongoDB)
```javascript
// Store sensor readings
{
    sensorId: "sensor_001",
    timestamp: ISODate("2023-06-15T10:30:00Z"),
    temperature: 25.5,
    humidity: 60,
    location: "Building A"
}

// Query recent readings
db.sensorData.find({
    sensorId: "sensor_001",
    timestamp: { $gte: ISODate("2023-06-15T00:00:00Z") }
}).sort({ timestamp: -1 }).limit(100);
```

### Event Sourcing (MongoDB)
```javascript
// Store all events
{
    eventId: ObjectId("..."),
    aggregateId: "order_123",
    eventType: "OrderPlaced",
    timestamp: ISODate("2023-06-15T10:30:00Z"),
    data: {
        customerId: "user_456",
        items: [...],
        total: 5000
    }
}
```

---

## 12. Best Practices

### MongoDB
✅ **Do:**
- Use indexes on frequently queried fields
- Limit document size (stay well below 16MB)
- Use aggregation pipeline for complex queries
- Implement proper error handling
- Use connection pooling

❌ **Avoid:**
- Large arrays that grow indefinitely
- Excessive embedding (deep nesting)
- Missing indexes on queries
- Using `SELECT *` equivalent (fetch only needed fields)
- Unbounded queries without limits

### Redis
✅ **Do:**
- Set expiration on cache keys
- Use appropriate data structures
- Monitor memory usage
- Use pipelining for bulk operations
- Implement cache invalidation strategy

❌ **Avoid:**
- Storing large values (keep under 1MB)
- Using Redis as primary database
- Keys without expiration
- Blocking operations in production

### General NoSQL
✅ **Do:**
- Understand your access patterns
- Design schema for reads, not writes
- Use denormalization strategically
- Monitor performance metrics
- Plan for data growth

❌ **Avoid:**
- Treating NoSQL like SQL (avoid complex joins)
- Over-normalizing data
- Ignoring consistency requirements
- Not planning for backups

---

## 13. Next Topics to Learn

### Intermediate
- **Transactions** in MongoDB (multi-document ACID)
- **Sharding** and horizontal scaling
- **Replication** and high availability
- **Change Streams** for real-time updates
- **Full-text search** in MongoDB

### Advanced
- **Data modeling** optimization
- **Performance tuning** and query optimization
- **Security** (authentication, authorization, encryption)
- **Backup and recovery** strategies
- **Multi-region deployment**

### Other NoSQL Databases
- **Elasticsearch**: Full-text search and analytics
- **Neo4j**: Graph relationships and traversals
- **DynamoDB**: AWS serverless NoSQL
- **Firebase**: Real-time mobile/web apps

---

## 14. SQL vs NoSQL: When to Use What

| Use SQL When | Use NoSQL When |
|--------------|----------------|
| Fixed schema | Dynamic/flexible schema |
| ACID required | Eventual consistency OK |
| Complex JOINs | Denormalized data |
| Transactions critical | High throughput needed |
| Structured data | Unstructured/semi-structured |
| Vertical scaling OK | Horizontal scaling required |

**Reality**: Most modern systems use **both**! SQL for critical transactions, NoSQL for caching, logs, and analytics.

---

## 15. Practice Resources

📚 **MongoDB University**: Free courses
🌐 **Redis University**: Interactive tutorials
💻 **Try Online**:
- MongoDB Atlas (free tier)
- Redis Labs (free tier)
- DB Fiddle (for practice)

**Next Steps**: Practice with real projects, experiment with different databases, and understand trade-offs for your specific use case!
