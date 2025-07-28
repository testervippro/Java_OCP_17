

## ✅ Overview: `java.io` vs `java.nio`

| Feature                 | `java.io`                         | `java.nio` / `java.nio2`                              |
| ----------------------- | --------------------------------- | ----------------------------------------------------- |
| Blocking I/O            | ✅ Yes                             | ❌ No (supports non-blocking)                          |
| Buffer-based            | ❌ No (stream-based)               | ✅ Yes (uses buffers)                                  |
| Selectors (event-based) | ❌ No                              | ✅ Yes (with `Selector`)                               |
| File operations         | Basic (`File`, `FileInputStream`) | Advanced (`Files`, `Path`, `AsynchronousFileChannel`) |
| Memory-mapped I/O       | ❌ No                              | ✅ Yes (`MappedByteBuffer`)                            |
| Channels support        | ❌ No                              | ✅ Yes (`FileChannel`, `SocketChannel`, etc.)          |
| Zero-copy               | ❌ No                              | ✅ Yes (efficient for large file transfers)            |

---

## 🔁 Steps to Migrate `java.io` → `java.nio`

---

### 1. 📖 Reading Files

#### ✅ `java.io` (blocking):

```java
BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
String line;
while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
reader.close();
```

#### 🔁 `java.nio` (non-blocking / efficient):

```java
List<String> lines = Files.readAllLines(Path.of("data.txt"));
lines.forEach(System.out::println);
```

✅ Or with streaming (lazy loading):

```java
try (Stream<String> stream = Files.lines(Path.of("data.txt"))) {
    stream.forEach(System.out::println);
}
```

---

### 2. 📝 Writing to a File

#### ✅ `java.io`:

```java
BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"));
writer.write("Hello World");
writer.close();
```

#### 🔁 `java.nio`:

```java
Files.write(Path.of("output.txt"), "Hello World".getBytes(), StandardOpenOption.CREATE);
```

---

### 3. ⚡ Efficient File Copy with `Channel`

#### 🔁 `java.nio`:

```java
try (FileChannel source = FileChannel.open(Path.of("source.txt"), StandardOpenOption.READ);
     FileChannel dest = FileChannel.open(Path.of("dest.txt"), StandardOpenOption.WRITE, StandardOpenOption.CREATE)) {

    source.transferTo(0, source.size(), dest);
}
```

---

If you want, I can also show:

* Socket migration (`java.net.Socket` to `SocketChannel`)
* Asynchronous file I/O with `AsynchronousFileChannel`
* Memory-mapped file example (`MappedByteBuffer`)


