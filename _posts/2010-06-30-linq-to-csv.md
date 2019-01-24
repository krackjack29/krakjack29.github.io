---
title: Linq to CSV
date: 2010-06-30 18:07:00 +530
layout: single
categories: 
   - .Net
tags:
   - linq
   - csharp
---
Its very common to import data from a csv file (flat file) into RDBMS or any such destination. When someone calls it a CSV file it need not be just a comma separated file, there could be much more to it.

A Csv file contains rows and columns which are differentiated by two main factors

1. Delimiter: Could be “comma” , “tab” or any other special character

2. TextQualifier: Field which are enclosed using this would be considered as a single field. e.g. {a,b,c,”def, is nice”,f} should result in only 5 columns and the 4th column should contain “def,is nice”. Usage of “ is common for these type of differentiation, ‘ and others could also be used.

Considering these criterias also in mind i have developed a simple linq to CSV extension, the code is as given below.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Text;
    using System.Text.RegularExpressions;
 
    /// <summary>
    /// Static class containing Utility function to parse the csv
    /// </summary>
    public static class CsvParser
    {
        /// <summary>
        /// Extension method to the StreamReader class which
        /// parses the csv and fetches the data
        /// </summary>
        /// <param name="source">The source.</param>
        /// <returns></returns>
        public static IEnumerable<IEnumerable<string>> Parse(this StreamReader source)
        {
            if (source == null)
            {
                throw new ArgumentException();
            }
            while (!source.EndOfStream)
            {
                StringBuilder line = new StringBuilder();
                GetCsvLine(source,line);
                yield return ParseLine(line);  
            }
        }
        
        /// <summary>
        /// Parses the line.
        /// </summary>
        /// <param name="line">The line.</param>
        /// <returns></returns>
        private static string[] ParseLine(StringBuilder line)
        {
            string[] arr = line.ToString().Split(SourceFileParameters.Instance.FieldSeparator.ToCharArray());
            //post process array
 
            List<string> columns = new List<string>();
            string tempString = string.Empty;
            bool inTextQualifier = false;
 
            foreach (string item in arr)
            {
                if (item.Contains(SourceFileParameters.Instance.TextQualifier))
                {
                    if (item.Split(SourceFileParameters.Instance.TextQualifier.ToCharArray()).Length % 2 == 0)
                    {
                        inTextQualifier = !inTextQualifier;
                    }
                }
                tempString += item;
                if (!inTextQualifier)
                {
                    columns.Add(tempString);
                    tempString = string.Empty;
                }
            }
 
            return columns.ToArray();
 
        }
 
        /// <summary>
        /// Gets the CSV line.
        /// </summary>
        /// <param name="reader">The reader.</param>
        /// <param name="line">The line.</param>
        private static void GetCsvLine(StreamReader reader, StringBuilder line)
        {
            line = line.Length == 0 ? line.Append(reader.ReadLine()) : line.Append("\n" + reader.ReadLine());
            Regex compareOpenStrings = new Regex("[" + SourceFileParameters.Instance.TextQualifier + "]");
            MatchCollection col = compareOpenStrings.Matches(line.ToString());
 
            if (col.Count % 2 == 1)
            {
                GetCsvLine(reader, line);
            }
            return;
        }
    }
```
It contains an extension method to the “StreamReader” class which can be used as follows

```csharp
        LinqToCsv.Delimiter = ",";
        LinqToCsv.TextQualifier = "\"";
            
        StreamReader reader = new StreamReader(@"D:\CSV\Appstrings.csv",Encoding.Default);
            
        var parsedData = reader.ParseCsv().Skip(100).Take(100);
 
        foreach (var item in parsedData)
        {
            foreach (var field in item)
            {
                Console.Write(field + "," );
            }
            Console.WriteLine("\n");
        }
```

The foreach loops would have the row and column information.