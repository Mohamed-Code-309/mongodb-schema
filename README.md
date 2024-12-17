# mongodb-schema


## Example: E-commerce Project

### [1] Collections:
#### 1. Users

```js
{
  _id: ObjectId,
  username: String,
  email: String,
  passwordHash: String,
  role: String, // e.g., 'admin', 'customer'
  createdAt: Date,
  updatedAt: Date
}
```
#### 2. Products
```js
{
  _id: ObjectId,
  name: String,
  description: String,
  price: Number,
  category: String,
  stock: Number,
  images: [String], // Array of image URLs
  createdAt: Date,
  updatedAt: Date
}
```
#### 3. Orders
```js
{
  _id: ObjectId,
  userId: ObjectId, // Reference to Users
  products: [
    {
      productId: ObjectId, // Reference to Products
      quantity: Number,
      price: Number
    }
  ],
  totalAmount: Number,
  status: String, // e.g., 'Pending', 'Shipped', 'Delivered'
  createdAt: Date,
  updatedAt: Date
}
```
#### 4. Categories
```js
{
  _id: ObjectId,
  name: String,
  description: String,
  parentCategory: ObjectId, // Reference to another Category
  createdAt: Date,
  updatedAt: Date
}
```
### [2] Indexing for Performance
- Index fields that are commonly queried, sorted, or filtered.
- Example Indexes:
  - **Users**: `email` (unique index)
  - **Products**: `name` (text index for search), `category` (index for filtering)
  - **Orders**: `userId` (index for lookup), `createdAt` (index for recent orders)


### [3] Horizontal Scaling
- Shard Keys: Identify shard keys for collections to distribute data across shards.
- Choose keys with high cardinality and low frequency of updates.
- Example: Use `email` in Users, `category` in Products.
- check How to shard data in mongodb: [Mongodb Sharding](https://github.com/Mohamed-Code-309/mongodb-sharding)

### [4] The Outlier Pattern
The Outlier Pattern in MongoDB is a schema design strategy used to handle scenarios where some keys in documents may grow in size and become over-sized. It solves this problem by splitting oversized or irregular data into separate collections or documents.

#### Example: Reviews Collection

Most products have a small number of reviews, but a few popular products might have thousands of reviews, which would make their documents disproportionately large.
```js
{
  _id: ObjectId_1,
  name: "Product A",
    reviews: [ 
    { userId: ObjectId, rating: 5, comment: "Awesome!" },
    ... thousands of reviews ...
  ],
  has_extras: true
}
```
```js
{
  _id: ObjectId_2,
  review_id: ObjectId_1,
  is_overFlow: true
    reviews: [ 
    { userId: ObjectId, rating: 3, comment: "good!" },
    ... rest of reviews ...
  ]
}
```
### [5] Common Mongodb Schema Design Rule:
1. Favor embedding over Referencing unless there is a compelling reason not to.
2. Avoid Joins and $Lookups if they can be avoided.
3. Arrays should not grown without bound. (Outlier Pattern)
4. How to model your data depends - entirely - on your particular application data access pattern.

