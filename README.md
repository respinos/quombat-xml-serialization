## How do you build a METS document in Java?

_Previously, on All our DOR: we had dataclasses that were passed to Jinja2 templates that created a METS2 XML._

This experiment is probably terribly organized and definitely an incomplete example.

## Option: Jackson-XML

[Jackson](https://github.com/FasterXML/jackson/) seems to be a popular way to un/marshal between POJOs and JSON, and can also [process XML](https://github.com/FasterXML/jackson-dataformat-xml).

The idea would be to define domain records (or DTOs, more likely :grimacing:) and annotate them:

```java
@JacksonXmlRootElement(localName = "book")
record Book(
    @JacksonXmlProperty() String title,
    @JacksonXmlProperty() int publicationYear,
    @JacksonXmlElementWrapper(localName="authors")
    @JacksonXmlProperty(localName="author") ArrayList<Author> authors
    ) {}
```

Marshalling this would result in an XML document that can be transformed to METS2 via XSLT.

(Why `ArrayList` there? It irks me to build XML documents upside down/inside out.)

## Option: JAXB

[JAXB](https://github.com/eclipse-ee4j/jaxb-ri) is a much more dense library that only deals with XML.

What is also can do is **compile XML schemas into Java classes**. That's what you'll find in `gov.loc.mets.v2`.

This _is_ a verbose approach ... but if the alternative is taking domain objects and instantiting Jackson-annotated-DTOs to transform...

:unamused:

Might as well take the domain objects and fill use the JAXB-derived classes.

## Why "quombat-xml-serialization"?

:face_with_head_bandage: Had multiple accidents trying to create a Gradle project in VSCode that could not be fixed by renaming directories so ... this worked.	 