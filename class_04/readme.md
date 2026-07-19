# MongoDB Lecture 2.1 — Joining Collections
### `$lookup`, `$unwind`, and `$replaceRoot` + `$mergeObjects`

---

## 1. Why Do We Need This?

MongoDB is a NoSQL database, so it doesn't have joins built in the same way SQL does. In SQL, if you have two related tables, you can join them directly in a single query. In MongoDB, related data usually lives in separate collections — for example, one collection for customers, and a completely separate one for their orders.

So the question becomes: how do we bring customer data and order data together, when they're sitting in two different collections?

This is exactly what these three stages solve, working together as a team:

- `$lookup` — brings related data from another collection into the current one
- `$unwind` — breaks apart the array that `$lookup` creates
- `$replaceRoot` + `$mergeObjects` — flattens everything into one clean, single-level document

We'll go through each one using the same real-world example: **customers and orders.**

---

## 2. Setting Up Sample Data

Before diving into the stages, let's create two collections and load some sample data. We'll use this exact same data for every example in this lecture, so you can actually see how the documents change shape after each stage.

**Create the `customers` collection:**

```javascript
db.customers.insertMany([
  { _id: 1, name: "Ali",   email: "ali@mail.com" },
  { _id: 2, name: "Sara",  email: "sara@mail.com" },
  { _id: 3, name: "Zain",  email: "zain@mail.com" }
])
```

**Create the `orders` collection:**

```javascript
db.orders.insertMany([
  { orderId: 101, customerId: 1, product: "Laptop",     amount: 90000 },
  { orderId: 102, customerId: 1, product: "Mouse",      amount: 1500 },
  { orderId: 103, customerId: 1, product: "Keyboard",   amount: 3000 },
  { orderId: 104, customerId: 2, product: "Headphones", amount: 5000 },
  { orderId: 105, customerId: 3, product: "Monitor",    amount: 25000 }
])
```

Notice how `customerId` in the `orders` collection matches up with `_id` in the `customers` collection. That matching relationship is exactly what `$lookup` is going to use to connect the two collections.

**A quick look at who has what:**

- **Ali** (`_id: 1`) has 3 orders — Laptop, Mouse, and Keyboard
- **Sara** (`_id: 2`) has 1 order — Headphones
- **Zain** (`_id: 3`) has 1 order — Monitor

Keep this in mind as we go — it'll help you predict exactly what each stage should produce before you even run it.

---

## 3. The `$lookup` Stage

`$lookup` is the stage that lets us pull data from one collection into another — kind of like a join in SQL, even though technically it isn't one.

**Example:**

```javascript
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customerId",
      as: "orders_data"
    }
  }
])
```

**Breaking down each part:**

- **`from`** — tells MongoDB which collection to pull data from. In this case, `"orders"`.
- **`localField`** — the field in our current collection (`customers`) that we want to match against. Here, that's `"_id"`.
- **`foreignField`** — the field in the other collection (`orders`) that should match the `localField`. Here, that's `"customerId"`.
- **`as`** — the name we're giving to the new field where all the matched data gets stored. Here, that's `"orders_data"`.

**What actually happens:**

For every single customer document, MongoDB looks into the `orders` collection and finds every order where `customerId` matches that customer's `_id`. All of those matching orders get collected and added into a brand new array field called `orders_data`, right inside that customer's document.

So if a customer has placed 3 orders, `orders_data` will be an array containing 3 order objects. If they've placed none, `orders_data` will just be an empty array.

**What the output looks like with our sample data:**

Running this on our `customers` and `orders` collections gives us something like this for Ali:

```javascript
{
  _id: 1,
  name: "Ali",
  email: "ali@mail.com",
  orders_data: [
    { orderId: 101, customerId: 1, product: "Laptop",   amount: 90000 },
    { orderId: 102, customerId: 1, product: "Mouse",    amount: 1500 },
    { orderId: 103, customerId: 1, product: "Keyboard", amount: 3000 }
  ]
}
```

Notice that Ali's document now has all 3 of his orders sitting inside a single array field, `orders_data`. Sara and Zain would look the same, except each of their `orders_data` arrays would only contain 1 order object, since they each placed only one order.

---

## 4. The `$unwind` Stage

Right after `$lookup`, `orders_data` is always an array — even if there's only one single order inside it. Arrays are a little inconvenient to work with directly, especially if you want to treat each order as its own document. That's exactly the problem `$unwind` solves.

**Example:**

```javascript
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customerId",
      as: "orders_data"
    }
  },
  {
    $unwind: "$orders_data"
  }
])
```

**What actually happens:**

If a customer has 3 orders sitting inside `orders_data`, `$unwind` will split that single customer document into 3 separate documents. All 3 of these documents will look identical in terms of customer information, except `orders_data` will now be a single order object in each one, instead of an array holding all 3.

In short: `$unwind` takes one document that has an array inside it, and turns it into multiple documents — one for each item that was in that array.

**What the output looks like with our sample data:**

Ali's single document from before now becomes 3 separate documents:

```javascript
{ _id: 1, name: "Ali", email: "ali@mail.com", orders_data: { orderId: 101, customerId: 1, product: "Laptop",   amount: 90000 } }
{ _id: 1, name: "Ali", email: "ali@mail.com", orders_data: { orderId: 102, customerId: 1, product: "Mouse",    amount: 1500 } }
{ _id: 1, name: "Ali", email: "ali@mail.com", orders_data: { orderId: 103, customerId: 1, product: "Keyboard", amount: 3000 } }
```

Notice `orders_data` is no longer an array — it's now just a single order object in each document. Sara and Zain, who only had 1 order each, won't be affected much by `$unwind`, since there was only ever one item in their arrays anyway. Ali is the clearest example here, since his one document turns into three.

---

## 5. The `$replaceRoot` + `$mergeObjects` Stage

Even after `$unwind`, we're not quite done. `orders_data` is still sitting as a nested object inside the document. The customer's fields (like name, email) and the order's fields (like orderId, amount) are technically still separate from each other — one is at the top level, and the other is nested one level deeper.

This last stage fixes that by flattening everything into one single, clean structure.

**Example:**

```javascript
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customerId",
      as: "orders_data"
    }
  },
  {
    $unwind: "$orders_data"
  },
  {
    $replaceRoot: {
      newRoot: { $mergeObjects: ["$orders_data", "$$ROOT"] }
    }
  }
])
```

**Breaking down each part:**

- **`$replaceRoot`** — replaces the entire structure of the document with something new that we provide. Whatever we place inside `newRoot` becomes the new document, completely replacing the old shape.
- **`$mergeObjects`** — takes two objects and merges their fields together into one single object.

Here, we're merging `orders_data` (the nested order object) with `$$ROOT`, where `$$ROOT` refers to the entire current document — including all the customer fields.

**What actually happens:**

The end result is one flat document containing both the customer's information and the order's information sitting together at the same level, with no nesting left at all.

**What the output looks like with our sample data:**

Take Ali's first order document from the `$unwind` step. After `$replaceRoot` + `$mergeObjects`, it flattens into this:

```javascript
{
  orderId: 101,
  customerId: 1,
  product: "Laptop",
  amount: 90000,
  _id: 1,
  name: "Ali",
  email: "ali@mail.com"
}
```

Compare this to the `$unwind` output above — `orders_data` as a nested field is completely gone now. Every field, whether it originally came from the customer or from the order, is sitting flat at the same top level. This same flattening happens for all of Ali's 3 orders, and for Sara's and Zain's single orders too, giving us 5 final flat documents in total (one for every order across all 3 customers).

---

## 6. Putting It All Together

Here's the complete pipeline, exactly as it would run in one go:

```javascript
db.customers.aggregate([
  // Stage 1 — bring in matching orders from the orders collection
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customerId",
      as: "orders_data"
    }
  },
  // Stage 2 — flatten (deconstruct) the orders_data array
  {
    $unwind: "$orders_data"
  },
  // Stage 3 — merge the nested orders_data object with the main document
  {
    $replaceRoot: {
      newRoot: { $mergeObjects: ["$orders_data", "$$ROOT"] }
    }
  }
])
```

**Thinking through it step by step:**

1. **Before this pipeline runs:** each customer document exists on its own, with no order information attached.
2. **After `$lookup`:** each customer document now has an extra field, `orders_data`, holding an array of all their matching orders.
3. **After `$unwind`:** instead of one customer document with an array of orders, you now get one separate document per order — each still containing the full customer details, plus one single order nested inside `orders_data`.
4. **After `$replaceRoot` + `$mergeObjects`:** the nested `orders_data` object gets merged directly into the document, so customer fields and order fields all sit together at the same flat level — no nesting, no arrays, just one clean document per order.

**The final output, using our sample data, looks like this — 5 flat documents, one per order:**

```javascript
{ orderId: 101, customerId: 1, product: "Laptop",     amount: 90000, _id: 1, name: "Ali",  email: "ali@mail.com" }
{ orderId: 102, customerId: 1, product: "Mouse",      amount: 1500,  _id: 1, name: "Ali",  email: "ali@mail.com" }
{ orderId: 103, customerId: 1, product: "Keyboard",   amount: 3000,  _id: 1, name: "Ali",  email: "ali@mail.com" }
{ orderId: 104, customerId: 2, product: "Headphones", amount: 5000,  _id: 2, name: "Sara", email: "sara@mail.com" }
{ orderId: 105, customerId: 3, product: "Monitor",    amount: 25000, _id: 3, name: "Zain", email: "zain@mail.com" }
```

This is the real power of the pipeline: we started with 3 customer documents and 5 separate order documents in two different collections, and ended up with 5 clean, flat, combined documents — each one showing exactly which customer placed which order, with all the details sitting together.

---
