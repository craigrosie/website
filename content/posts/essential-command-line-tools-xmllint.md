---
title: "Essential Command Line Tools: xmllint"
date: 2018-01-31
tags: ["command line", "xml"]
draft: false
---
_I currently spend a lot of my time at the command line, so I thought I'd start a series on tools that are essential to my workflow. Some of these will be well known tools, and others less so, but hopefully there's something useful for everyone!_

# xmllint

## What is it?

xmllint is a command line XML tool.

## How do I get it?

It's built in to MacOS (ships with the `libxml2` package) 🎉

## Why is it useful?

Do you ever find yourself reading XML formatted like this?

```xml
<?xml version="1.0" encoding="UTF-8"?> <shiporder orderid="889923" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="shiporder.xsd"> <orderperson>John Smith</orderperson> <shipto> <name>Ola Nordmann</name> <address>Langgt 23</address> <city>4000 Stavanger</city> <country>Norway</country> </shipto> <item> <title>Empire Burlesque</title> <note>Special Edition</note> <quantity>1</quantity> <price>10.90</price> </item> <item> <title>Hide your heart</title> <quantity>1</quantity> <price>9.90</price> </item> </shiporder>
```
_Example XML taken from [W3Schools][w3]_

Now imagine working on a 100+mb XML file with over 100mil characters... If only we could format the XML nicely, it might be a bit easier to read.

This is where `xmllint` comes in:

```bash
xmllint --format example.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<shiporder xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" orderid="889923" xsi:noNamespaceSchemaLocation="shiporder.xsd">
  <orderperson>John Smith</orderperson>
  <shipto>
    <name>Ola Nordmann</name>
    <address>Langgt 23</address>
    <city>4000 Stavanger</city>
    <country>Norway</country>
  </shipto>
  <item>
    <title>Empire Burlesque</title>
    <note>Special Edition</note>
    <quantity>1</quantity>
    <price>10.90</price>
  </item>
  <item>
    <title>Hide your heart</title>
    <quantity>1</quantity>
    <price>9.90</price>
  </item>
</shiporder>
```

Much easier to read!

And of course, since `xmllint` is used via the command line, you can use all the other tools and tricks available to you to do further processing:

```bash
xmllint --format example.xml > formatted.xml  # Write formatted output to new file

xmllint --format example.xml | wc -l  # Get number of lines in the XML document

xmllint --format example.xml | grep -A 3 -B 1 "Hide your heart"  # Find details of item called "Hide your heart"
```

[w3]: https://www.w3schools.com/xml/schema_example.asp
