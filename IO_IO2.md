
---

## 🔁 Migration: `java.io` → `java.nio` (Java 17)

---

### ✅ 1. **Reading a File**

#### `java.io`:

```java
BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
String line;
while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
reader.close();
```

#### 🔁 `java.nio`:

```java
// Eager (read all at once)
List<String> lines = Files.readAllLines(Path.of("data.txt"));
lines.forEach(System.out::println);

// Or Lazy (stream-based)
try (Stream<String> stream = Files.lines(Path.of("data.txt"))) {
    stream.forEach(System.out::println);
}
```

---

### ✅ 2. **Writing to a File**

#### `java.io`:

```java
BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"));
writer.write("Hello World");
writer.close();
```

#### 🔁 `java.nio`:

```java
Files.write(Path.of("output.txt"), "Hello World".getBytes(), StandardOpenOption.CREATE);
```

📌 **Options**:

* `CREATE`: create if not exists
* `APPEND`: add to end of file
* `TRUNCATE_EXISTING`: overwrite
* `WRITE`: open for writing

👉 Example with `APPEND`:

```java
Files.write(Path.of("output.txt"), "Another line\n".getBytes(), StandardOpenOption.CREATE, StandardOpenOption.APPEND);
```

---

### ✅ 3. **Copy a File**

#### `java.io`:

```java
InputStream in = new FileInputStream("source.txt");
OutputStream out = new FileOutputStream("dest.txt");
in.transferTo(out);
in.close();
out.close();
```

#### 🔁 `java.nio`:

```java
Files.copy(Path.of("source.txt"), Path.of("dest.txt"), StandardCopyOption.REPLACE_EXISTING);
```

📌 You can also use **`FileChannel`** for efficient transfer:

```java
try (FileChannel source = FileChannel.open(Path.of("source.txt"), StandardOpenOption.READ);
     FileChannel dest = FileChannel.open(Path.of("dest.txt"), StandardOpenOption.WRITE, StandardOpenOption.CREATE)) {

    source.transferTo(0, source.size(), dest);
}
```

---

### ✅ 4. **Delete a File or Directory**

```java
// Simple delete
Files.delete(Path.of("file.txt"));

// Delete if exists
Files.deleteIfExists(Path.of("file.txt"));
```

---

### ✅ 5. **Delete Directory Recursively**

```java
Path dir = Path.of("myDir");

Files.walk(dir)
     .sorted(Comparator.reverseOrder()) // Delete children before parent
     .forEach(path -> {
         try {
             Files.delete(path);
         } catch (IOException e) {
             e.printStackTrace();
         }
     });
```

---

### ✅ 6. **Create Directory or File**

```java
Files.createDirectory(Path.of("newDir"));
Files.createDirectories(Path.of("nested/dir/path"));
Files.createFile(Path.of("file.txt"));
```

---

### ✅ 7. **Check Existence**

```java
Files.exists(Path.of("somefile.txt"));
Files.notExists(Path.of("somefile.txt"));
Files.isDirectory(Path.of("mydir"));
```

---

### ✅ 8. **Walk Through Files (Recursive Search)**

```java
try (Stream<Path> paths = Files.walk(Path.of("myDir"))) {
    paths.filter(Files::isRegularFile)
         .filter(p -> p.toString().endsWith(".txt"))
         .forEach(System.out::println);
}
```

---

### ✅ 9. **Stream-Based Loops (for-loop → stream)**

```java
// Traditional loop
for (String line : lines) {
    System.out.println(line);
}

// With streams
lines.stream().forEach(System.out::println);
```

---

## 🚀 Bonus: Advanced I/O (optional)

### 🔸 Asynchronous File I/O

```java
AsynchronousFileChannel asyncChannel = AsynchronousFileChannel.open(
    Path.of("async.txt"), StandardOpenOption.WRITE, StandardOpenOption.CREATE);

ByteBuffer buffer = ByteBuffer.wrap("Hello async".getBytes());
asyncChannel.write(buffer, 0);
```

---

### 🔸 Memory-Mapped File

```java
try (FileChannel channel = FileChannel.open(Path.of("bigfile.dat"), StandardOpenOption.READ)) {
    MappedByteBuffer buffer = channel.map(FileChannel.MapMode.READ_ONLY, 0, channel.size());
    while (buffer.hasRemaining()) {
        System.out.print((char) buffer.get());
    }
}
```


