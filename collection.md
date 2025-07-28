

## ✅ Goal

Compare **Java collection types** by:

1. 📚 Collection Type (List, Set, Map, etc.)
2. 🧠 **Big-O Complexity** (for common operations)
3. 🔒 **Mutability** (mutable vs. immutable)
4. ⚠️ **Null Handling** (can store null keys/values?)

---

## 📊 Comparison Table

| Collection Type                   | Big-O Add          | Big-O Get | Big-O Remove   | Immutable Support   | Null Handling                          |
| --------------------------------- | ------------------ | --------- | -------------- | ------------------- | -------------------------------------- |
| **ArrayList**                     | O(1) amortized     | O(1)      | O(n)           | Yes (via `List.of`) | ✅ Allows `null` values                 |
| **LinkedList**                    | O(1) addFirst/Last | O(n)      | O(1) from ends | Yes                 | ✅ Allows `null` values                 |
| **HashSet**                       | O(1)               | N/A       | O(1)           | Yes (via `Set.of`)  | ✅ Allows **one** `null` element        |
| **LinkedHashSet**                 | O(1)               | N/A       | O(1)           | No                  | ✅ Allows `null`                        |
| **TreeSet**                       | O(log n)           | N/A       | O(log n)       | No                  | ❌ Throws `NullPointerException`        |
| **HashMap**                       | O(1)               | O(1)      | O(1)           | Yes (via `Map.of`)  | ✅ 1 `null` key, multiple `null` values |
| **LinkedHashMap**                 | O(1)               | O(1)      | O(1)           | No                  | ✅ Same as HashMap                      |
| **TreeMap**                       | O(log n)           | O(log n)  | O(log n)       | No                  | ❌ `null` key not allowed               |
| **ConcurrentHashMap**             | O(1)               | O(1)      | O(1)           | No                  | ❌ `null` keys/values not allowed       |
| **Immutable List/Set/Map (`of`)** | N/A                | O(1)      | ❌ Unsupported  | ✅ Yes               | ❌ `null` not allowed (throws NPE)      |



## 🧠 1. Big-O Time Complexity Summary

| Operation      | ArrayList | LinkedList | HashSet | TreeSet  | HashMap | TreeMap  |
| -------------- | --------- | ---------- | ------- | -------- | ------- | -------- |
| Add            | O(1)      | O(1)       | O(1)    | O(log n) | O(1)    | O(log n) |
| Get (by index) | O(1)      | O(n)       | —       | —        | O(1)    | O(log n) |
| Remove         | O(n)      | O(1) ends  | O(1)    | O(log n) | O(1)    | O(log n) |

---

A Red-Black Tree is a type of self-balancing binary search tree (BST).
Java’s TreeMap and TreeSet use Red-Black Trees under the hood to maintain sorted order and guarantee O(log n) performance for insertion, deletion, and lookup.


## 🔒 2. Immutable Collections

| Method                            | Description              | `null` allowed?            |
| --------------------------------- | ------------------------ | -------------------------- |
| `List.of(...)`                    | Immutable list (Java 9+) | ❌ Throws NPE               |
| `Set.of(...)`                     | Immutable set            | ❌ Throws NPE               |
| `Map.of(k, v, ...)`               | Immutable map (Java 9+)  | ❌ NPE for null key/value   |
| `Collectors.toUnmodifiableList()` | Immutable via Stream     | ❌ NPE if stream has `null` |

---

## ⚠️ 3. Null Handling (Detailed)

| Collection               | `null` key | `null` value            | Notes                    |
| ------------------------ | ---------- | ----------------------- | ------------------------ |
| `ArrayList`              | N/A        | ✅ Yes                   | Multiple `null`s allowed |
| `HashSet`                | ✅ One      | ✅ Yes                   | Only one `null` allowed  |
| `TreeSet`                | ❌ No       | ✅ (via comparator only) | NPE if no comparator     |
| `HashMap`                | ✅ One      | ✅ Yes                   | Only one `null` key      |
| `TreeMap`                | ❌ No       | ✅ Yes                   | NPE on `null` key        |
| `ConcurrentHashMap`      | ❌ No       | ❌ No                    | Rejects all `null`       |
| `Immutable List/Set/Map` | ❌ No       | ❌ No                    | Created with `of(...)`   |

---

## ✅ Sample Code Snippets

### ✅ HashMap allows `null`

```java
Map<String, String> map = new HashMap<>();
map.put(null, "NULL_KEY");
map.put("key", null);
System.out.println(map); // prints {null=NULL_KEY, key=null}
```

### ❌ TreeMap with `null` key throws error

```java
Map<String, String> map = new TreeMap<>();
map.put(null, "Oops"); // NullPointerException
```

### ❌ `List.of(null)` fails

```java
List<String> list = List.of(null); // Throws NullPointerException
```

---

## 🧠 Conclusion

| Comparison Point        | Best for Performance | Allows `null`?         | Immutable Variant      |
| ----------------------- | -------------------- | ---------------------- | ---------------------- |
| `HashMap`               | ✅ O(1)               | ✅ (1 key, many values) | `Map.of()` ❌ null      |
| `TreeMap`               | ❌ Slower             | ❌ No key               | ❌ No immutable version |
| `ArrayList`             | ✅ O(1) for get/add   | ✅ Yes                  | `List.of()` ❌ null     |
| `HashSet`               | ✅ O(1) add           | ✅ Only 1               | `Set.of()` ❌ null      |
| `Immutable Collections` | ❌ Cannot change      | ❌ No `null` allowed    | ✅ Safe and efficient   |

---

