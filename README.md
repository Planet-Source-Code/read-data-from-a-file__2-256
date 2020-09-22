<div align="center">

## Read data from a file


</div>

### Description

There are a number of ways to read data from a file. If you're reading a file as raw binary data, you open a file using a FileInputStream(String) constructor and use one of the various read() methods to read the data into an array of bytes. For example the following program reads raw data from a file specified on the command line. It then writes the same data to the standard output.

(Java FAQ:found on the web at:http://sunsite.unc.edu/javafaq/javafaq.html)
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[N/A](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/empty.md)
**Level**          |Unknown
**User Rating**    |3.6 (18 globes from 5 users)
**Compatibility**  |Java \(JDK 1\.1\)
**Category**       |[Input/ Output](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/input-output__2-84.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/read-data-from-a-file__2-256/archive/master.zip)





### Source Code

```
import java.io.*;
class ReadRawData {
 public static void main (String args[]) {
  boolean done = false;
  byte b[] = new byte[1024];
  int num_bytes = 0;
  FileInputStream fin = null;
  try {
   fin = new FileInputStream(args[0]);
  }
  catch(ArrayIndexOutOfBoundsException e) {
   System.out.println("You have to give me the name of a file to open.");
   System.exit(0);
  }
  catch (FileNotFoundException e) {
   System.out.println("Could not open input file " + args[0]);
   System.exit(0);
  }
  catch(IOException e) {
   System.out.println("Error while opening input file" + args[0]);
   System.exit(0);
  }
  catch (Exception e) {
   System.out.println("Unexpected exception: " + e);
   System.exit(0);
  }
  try {
   num_bytes = fin.read(b);
  }
  catch(IOException e) {
   System.out.println("Finished Reading: " + e);
   done = true;
  }
  catch (Exception e) {
   System.out.println("Unexpected exception: " + e);
   System.exit(0);
  }
  while(!done) {
   System.out.write(b, 0, num_bytes);
   try {
    num_bytes = fin.read(b);
   }
   catch(IOException e) {
    System.out.println("Finished Reading: " + e);
    done = true;
   }
   catch (Exception e) {
    System.out.println("Unexpected exception: " + e);
    System.exit(0);
   }
   if (num_bytes == -1) done = true;
  } // end while
 } // end main
} // end ReadRawData
On the other hand if you're reading a text file in Java 1.0 you'll probably want to use a DataInputStream which gives you a readLine() method that returns successive lines of the file as Java Strings. You can then process each String as you see fit.
// Implement the Unix cat utility in java
import java.io.*;
class cat {
 public static void main (String args[]) {
  String thisLine;
  //Loop across the arguments
  for (int i=0; i < args.length; i++) {
   //Open the file for reading
   try {
    FileInputStream fin = new FileInputStream(args[i]);
    try {
     DataInputStream myInput = new DataInputStream(fin);
     try {
      while ((thisLine = myInput.readLine()) != null) { // while loop begins here
       System.out.println(thisLine);
      } // while loop ends here
     }
     catch (Exception e) {
      System.out.println("Error: " + e);
     }
   } // end try
   catch (Exception e) {
    System.out.println("Error: " + e);
   }
  } // end try
  catch (Exception e) {
   System.out.println("failed to open file " + args[i]);
   System.out.println("Error: " + e);
  }
 } // for ends here
} // main ends here
}
This code emulates the Unix "cat" command. Given a series of filenames on the command line it concatenates the files onto the standard output.
In Java 1.1 DataInputStream.readLine() is deprecated. You should use a BufferedReader instead as in this class:
// Implement the Unix cat utility in java
import java.io.*;
class cat {
 public static void main (String args[]) {
  String thisLine;
  //Loop across the arguments
  for (int i=0; i < args.length; i++) {
   //Open the file for reading
   try {
    FileReader fr = new FileReader(args[i]);
    BufferedReader myInput = new BufferedReader(fr);
    while ((thisLine = myInput.readLine()) != null) { // while loop begins here
     System.out.println(thisLine);
    } // while loop ends here
   } // end try
   catch (IOException e) {
    System.out.println("Error: " + e);
   }
 } // for ends here
} // main ends here
}
```

