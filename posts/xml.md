---
title: '[Selenium] XML vs HTML'
date: '2024-10-18'
---
# 

XML and HTML are commonly used markup languages. They share similar names but serve distinct purposes. HTML is used to display text, images, buttons, checkboxes, and dropdowns on a website, with a specific focus on the appearance of data. Conversely, ***XML facilitates data exchange and storage, encoding data in a format readable by both machines and humans.*** Designed to be software and hardware independent, XML is a versatile tool for transporting and storing data in a legible format.

## Main Differences
The fundamental difference between HTML and XML lies in their tags. XML tags are not predefined like those in HTML; instead, they are specified by the creator of the XML document according to their desired standards. Creators typically use descriptive words that reflect the data, to make the XML schema self-descriptive and easy to understand. Applications using the same XML schema can interpret the format consistently, allowing data sharing across different systems. XML's ability to separate data from its presentation means that the same XML data can be displayed in various ways (by HTML, for example).

## Why XML?
XML enables different applications to exchange and store data along with its structure in a universally comprehensible format. Its primary purpose is to facilitate data exchange among diverse systems, such as databases, by describing data in a structured tree format which defines specific relationships between elements.

## What is an XML Element?
An XML element encompasses everything from its start tag to its end tag.
Example:
```xml
<price>10.00</price>
```
Elements may contain text, attributes, other elements, or a combination above.

## Empty XML Elements
An element with no content is considered empty. This can be indicated in XML in two ways:
```xml
<element></element>
```
Or through a self-closing tag:
```xml
<element/>
```

## Attributes
XML elements can have multiple attributes, offering flexibility in data structuring. Consider the following examples:

```xml
<person sex="female">
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
</person>

<person>
  <sex>female</sex>
  <firstname>Anna</firstname>
  <lastname>Smith</lastname>
</person>
```
In the first example, `sex` is an attribute; in the second, it is a child element. While both examples convey the same information, attributes usually add extra details that help explain the element's data. Although attributes are less flexible than elements (e,g., they cannot contain multiple values or tree structures and are less adaptable), they are suitable for simple, descriptive details like an ID.

The following example is very bad:
```xml
<person sex="female" firstname='Anna' lastname='Smith'>
</person>
```
