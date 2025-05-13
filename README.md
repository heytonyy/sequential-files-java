# Java Sequential Files Study Guide

## Table of Contents

1. [Introduction to Sequential Files](#introduction-to-sequential-files)
2. [File Streams Overview](#file-streams-overview)
3. [Character Streams](#character-streams)
4. [Byte Streams](#byte-streams)
5. [File Operations](#file-operations)
6. [Best Practices](#best-practices)
7. [Example Code](#example-code)
8. [Practice Questions](#practice-questions)

## Introduction to Sequential Files

Sequential files in Java are files that are read or written in a sequential manner, from beginning to end. Unlike random access files, you cannot jump to arbitrary positions within the file. Java SDK 8 provides several classes for working with sequential files.

### Key Concepts:
- Sequential access means reading/writing data in order
- Data is processed one element at a time
- Common for log files, text files, and simple data storage
- Java provides different stream types for different data formats

## File Streams Overview

Java uses streams for file I/O operations. Streams can be categorized into:

### Character Streams
- Process text data (characters)
- Use Unicode encoding
- Suitable for text files
- Base classes: `Reader` and `Writer`

### Byte Streams
- Process binary data (bytes)
- Work with any type of file
- More fundamental than character streams
- Base classes: `InputStream` and `OutputStream`

## Character Streams

### FileReader and FileWriter

#### FileReader
```java
FileReader reader = new FileReader("input.txt");
int character = reader.read(); // Reads one character
reader.close();
```

#### FileWriter
```java
FileWriter writer = new FileWriter("output.txt");
writer.write("Hello, World!");
writer.close();
```

### BufferedReader and BufferedWriter

#### BufferedReader
- Improves performance by buffering characters
- Provides convenient `readLine()` method

```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
String line = br.readLine();
br.close();
```

#### BufferedWriter
- Buffers characters for efficient writing
- Use `flush()` to ensure data is written

```java
BufferedWriter bw = new BufferedWriter(new FileWriter("file.txt"));
bw.write("Line 1");
bw.newLine();
bw.flush();
bw.close();
```

## Byte Streams

### FileInputStream and FileOutputStream

#### FileInputStream
```java
FileInputStream fis = new FileInputStream("data.bin");
int bytesRead = fis.read(); // Reads one byte
fis.close();
```

#### FileOutputStream
```java
FileOutputStream fos = new FileOutputStream("output.bin");
fos.write(65); // Writes byte value 65 (ASCII 'A')
fos.close();
```

### DataInputStream and DataOutputStream

#### DataInputStream
- Reads primitive data types from binary files
- Provides methods like `readInt()`, `readDouble()`

```java
DataInputStream dis = new DataInputStream(new FileInputStream("data.bin"));
int value = dis.readInt();
double price = dis.readDouble();
dis.close();
```

#### DataOutputStream
- Writes primitive data types to binary files
- Provides methods like `writeInt()`, `writeDouble()`

```java
DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.bin"));
dos.writeInt(100);
dos.writeDouble(99.99);
dos.close();
```

## File Operations

### Creating Files
```java
File file = new File("newfile.txt");
if (!file.exists()) {
    file.createNewFile();
}
```

### Checking File Properties
```java
File file = new File("example.txt");
System.out.println("Exists: " + file.exists());
System.out.println("Can read: " + file.canRead());
System.out.println("Can write: " + file.canWrite());
System.out.println("Size: " + file.length() + " bytes");
```

### Try-with-resources (Java 7+)
```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

## Best Practices

1. **Always close resources**: Use try-with-resources or explicitly close streams
2. **Use buffered streams**: For better performance with large files
3. **Handle exceptions**: Use try-catch blocks for IOException
4. **Choose appropriate stream type**: Character streams for text, byte streams for binary
5. **Use specific data streams**: DataInputStream/OutputStream for primitive types

## Example Code

### Complete Example: Reading and Writing Text File
```java
import java.io.*;

public class FileExample {
    public static void main(String[] args) {
        // Writing to file
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("example.txt"))) {
            writer.write("Line 1");
            writer.newLine();
            writer.write("Line 2");
            writer.newLine();
            writer.write("Line 3");
        } catch (IOException e) {
            System.err.println("Error writing file: " + e.getMessage());
        }
        
        // Reading from file
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            int lineNumber = 1;
            while ((line = reader.readLine()) != null) {
                System.out.println(lineNumber + ": " + line);
                lineNumber++;
            }
        } catch (IOException e) {
            System.err.println("Error reading file: " + e.getMessage());
        }
    }
}
```

### Example: Binary File Operations
```java
import java.io.*;

public class BinaryFileExample {
    public static void main(String[] args) {
        // Writing binary data
        try (DataOutputStream dos = new DataOutputStream(
                new FileOutputStream("data.bin"))) {
            dos.writeInt(12345);
            dos.writeDouble(98.6);
            dos.writeBoolean(true);
            dos.writeUTF("Hello");
        } catch (IOException e) {
            System.err.println("Error writing binary file: " + e.getMessage());
        }
        
        // Reading binary data
        try (DataInputStream dis = new DataInputStream(
                new FileInputStream("data.bin"))) {
            int number = dis.readInt();
            double temperature = dis.readDouble();
            boolean flag = dis.readBoolean();
            String message = dis.readUTF();
            
            System.out.println("Number: " + number);
            System.out.println("Temperature: " + temperature);
            System.out.println("Flag: " + flag);
            System.out.println("Message: " + message);
        } catch (IOException e) {
            System.err.println("Error reading binary file: " + e.getMessage());
        }
    }
}
```

## Practice Questions

### Question 1
Which class should you use to read text from a file with better performance?

A) FileReader  
B) BufferedReader  
C) InputStreamReader  
D) Scanner  

**Answer: B) BufferedReader**
*BufferedReader provides buffering which improves performance for reading text files.*

### Question 2
What method is used to read an entire line from a text file?

A) read()  
B) readChar()  
C) readLine()  
D) readString()  

**Answer: C) readLine()**
*The readLine() method is provided by BufferedReader to read entire lines.*

### Question 3
Which statement about sequential files is correct?

A) You can access any position in the file randomly  
B) Data must be read in the order it was written  
C) They are faster than random access files  
D) They use more memory than random access files  

**Answer: B) Data must be read in the order it was written**
*Sequential files require reading data in the same order it was written.*

### Question 4
What is the base class for all byte input streams?

A) Reader  
B) InputStream  
C) FileInputStream  
D) BufferedInputStream  

**Answer: B) InputStream**
*InputStream is the abstract base class for all byte input streams.*

### Question 5
Which code correctly reads a text file line by line?

A)
```java
FileReader fr = new FileReader("file.txt");
fr.readLine();
```

B)
```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
String line = br.readLine();
```

C)
```java
FileInputStream fis = new FileInputStream("file.txt");
fis.readLine();
```

D)
```java
Scanner sc = new Scanner("file.txt");
sc.nextLine();
```

**Answer: B)**
*BufferedReader provides the readLine() method and is the correct way to read text files line by line.*

### Question 6
What happens if you don't close a file stream?

A) The program will crash  
B) Memory leak may occur  
C) The file becomes corrupted  
D) Nothing happens  

**Answer: B) Memory leak may occur**
*Not closing streams can lead to resource leaks and potential memory issues.*

### Question 7
Which pair of classes is used for binary I/O operations?

A) FileReader and FileWriter  
B) BufferedReader and BufferedWriter  
C) FileInputStream and FileOutputStream  
D) CharReader and CharWriter  

**Answer: C) FileInputStream and FileOutputStream**
*These classes are used for binary file operations, reading and writing bytes.*

### Question 8
What does the flush() method do?

A) Closes the stream  
B) Forces buffered data to be written  
C) Clears the buffer  
D) Reads data from buffer  

**Answer: B) Forces buffered data to be written**
*flush() ensures that any buffered data is written to the underlying stream.*