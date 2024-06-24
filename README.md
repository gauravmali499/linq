To create a zip file containing multiple `.bak` files, you can modify the previous example to loop through a list of `.bak` file paths and add each one to the zip archive. Here is how you can achieve that:

```csharp
using System;
using System.IO;
using System.IO.Compression;

class Program
{
    static void Main()
    {
        // List of .bak files to be zipped
        string[] bakFilePaths = new string[]
        {
            @"C:\path\to\your\file1.bak",
            @"C:\path\to\your\file2.bak",
            @"C:\path\to\your\file3.bak"
        };
        
        string zipFilePath = @"C:\path\to\your\output.zip";

        CreateZipFromBakFiles(bakFilePaths, zipFilePath);
    }

    static void CreateZipFromBakFiles(string[] bakFilePaths, string zipFilePath)
    {
        try
        {
            // Delete the zip file if it already exists
            if (File.Exists(zipFilePath))
            {
                File.Delete(zipFilePath);
            }

            // Create the zip file and add the .bak files to it
            using (FileStream zipToOpen = new FileStream(zipFilePath, FileMode.Create))
            {
                using (ZipArchive archive = new ZipArchive(zipToOpen, ZipArchiveMode.Create))
                {
                    foreach (string bakFilePath in bakFilePaths)
                    {
                        // Ensure each .bak file exists before adding it
                        if (File.Exists(bakFilePath))
                        {
                            archive.CreateEntryFromFile(bakFilePath, Path.GetFileName(bakFilePath));
                        }
                        else
                        {
                            Console.WriteLine($"The specified .bak file does not exist: {bakFilePath}");
                        }
                    }
                }
            }

            Console.WriteLine("Zip file created successfully.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"An error occurred: {ex.Message}");
        }
    }
}
```

### Explanation:
1. **Paths Array:** Define an array of strings where each string is the path to a `.bak` file you want to include in the zip.
2. **File Existence Check:** For each `.bak` file in the array, check if the file exists before attempting to add it to the zip archive.
3. **Zip Archive Creation:** Create the zip file and use a loop to add each `.bak` file from the array to the archive.

### Notes:
- This code will ensure that each specified `.bak` file exists before attempting to add it to the zip archive, logging a message if any file does not exist.
- The zip file is created in `Create` mode, meaning any existing file with the same name will be overwritten.
- Adjust the file paths and list of `.bak` files according to your needs.

This approach allows you to zip multiple `.bak` files into a single zip file efficiently.