# The GEDCOM X Conceptual Model

## Status

This document specifies a standard conceptual model for exchanging genealogical data,
and requests discussion and suggestions for improvements.

The current state of this document is as a DRAFT, and as such, the document
may be subject to changes, including backwards-incompatible changes, according to the
discussion and suggestions for improvement.

## Copyright Notice

Copyright 2011-2012 Intellectual Reserve, Inc.

## License

This document is distributed under a Creative Commons Attribution-ShareAlike license.
For details, see:

http://creativecommons.org/licenses/by-sa/3.0/

# 1. Introduction

The GEDCOM X conceptual model is a specification of formal concepts and types
that are used to provide a standard model and vocabulary for describing genealogical
data. Genealogical data is structured by data types such as persons, 
relationships, and sources.

[![Conceptual Model Graph](https://github.com/atomkirk/gedcomx/blob/master/specifications/support/conceptual-model-graph.png?raw=true)](https://github.com/atomkirk/gedcomx/blob/master/specifications/support/conceptual-model-graph.pdf?raw=true)

## 1.1 Identifier, Version, and Dependencies

The identifier for this specification is:

`http://gedcomx.org/conceptual-model/v1`

For convenience, the GEDCOM X conceptual model may be referred to as "GEDCOM X 1.0".
This specification uses "GEDCOM X" internally.

This specification is depends on the GEDCOM X Date Model specification identified
by [`http://gedcomx.org/date-model/v1`](https://github.com/FamilySearch/gedcomx/blob/master/specifications/date-model-specification.md).

This specification refers to the GEDCOM X Event Types specification identified
by [`http://gedcomx.org/event-types/v1`](https://github.com/FamilySearch/gedcomx/blob/master/specifications/event-types-specification.md)
to recommend event types to be used.

This specification refers to the GEDCOM X Fact Types specification identified
by [`http://gedcomx.org/fact-types/v1`](https://github.com/FamilySearch/gedcomx/blob/master/specifications/fact-types-specification.md)
to recommend fact types to be used.

This specification refers to the GEDCOM X Name Part Qualifiers specification identified
by [`http://gedcomx.org/name-part-qualifiers/v1`](https://github.com/FamilySearch/gedcomx/blob/master/specifications/name-part-qualifiers-specification.md)
to recommend name part qualifiers to be used.

## 1.2 Notational Conventions

### 1.2.1 Keywords

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14,
[RFC2119](http://tools.ietf.org/html/rfc2119), as scoped to those conformance
targets.

### 1.2.2 Data Types and Data Instances

This specification uses the term "data type" to refer to a formal description of
a data structure, including the properties that define valid instances of the
data type. For example, information about a person might be contained within a 
data structure that supplies the person's name, birth date, and gender. A "data 
type" defines the formal properties of the data structure.

When a property of a data type is specified as being of a particular data type, the
property inherits the corresponding requirements from that data type's definition.
When a data type is specified as an extension of another data type, the extending
data type inherits the corresponding requirements from the extended data type's
definition.

Data types are defined by the following sections:

1. The *identifier* for the data type, which takes the form of a URI.
2. The *extension* of the data type (if any) which specifies which data type
   is extended by the data type.
3. The *properties* of the data type, which define the information the data type
   encapsulates.

This specification uses the term "data instance" to refer to the notion of a particular
instance, or instantiation, of a data type.

<a id="uri"/>

### 1.2.3 Controlled Vocabularies

GEDCOM X defines a set of controlled vocabularies for the purposes of identifying the semantic
context of data instances. A controlled vocabulary is often defined to create a set
of "known types" of data instances such as facts, names, genders, etc. For example, a controlled
vocabulary is specified to identify the set of known types of events so applications
can share semantic context of the event.

Elements of a controlled vocabulary are identified by an enumerated value.

## 1.2.4 The URI Reference

The Uniform Resource Identifier ("URI") is fundamental to the GEDCOM X conceptual model.
The URI is used to identify both the data types and data instances. The
URI is also used to identify elements of the controlled vocabularies defined by GEDCOM X.
The URI is specified by [RFC 3986](http://tools.ietf.org/html/rfc3986).

GEDCOM X resources use the URI to reference other entities. For example, a GEDCOM X `Relationship`
identifies a person in the relationship by referencing a URI that identifies the person. When a property
(such as the `person1` property of `Relationship`) is of type `URI`, the value of the property
is interpreted as a "URI Reference" as defined by [RFC 3986, section 4](http://tools.ietf.org/html/rfc3986#section-4).
This specification uses the term "URI" to refer to both a "URI" and a "URI Reference" as
defined by [RFC 3986](http://tools.ietf.org/html/rfc3986).

<a id="formal-values" />

### 1.2.5 Original and Normalized Values

When a property is identified as an "original value", the value of the property
is interpreted as the literal value supplied by a user. If a property is identified as a
"normalized value", the value of the property is assumed to be formally formatted (either by
a user or by the application) for the purpose of easier processing, such as for display
purposes.

<a name="enumerated-value"/>

### 1.2.6 Enumerated Values

Enumerated values are used throughout GEDCOM X to constrain values of properties to a limited (though
not necessarily small) number of possibilities. Enumerated values are used to ensure portability and must
be a discrete, machine-identifiable value based on a specific specification.  Enumerated values take the
form of a URI. Members of controlled vocabularies are enumerated values.

The base URI for enumerated values defined by GEDCOM X is `http://gedcomx.org/`.

Enumerated values which are not defined directly or indirectly from this specification SHOULD be declared in a
freely-distributable specification and MUST NOT use the value `http://gedcomx.org/` as a base URI.

## 1.3 Internationalization Considerations

GEDCOM X must be designed to accommodate users and software of different languages and locales.
To this end, a property named `lang` is supported on relevant GEDCOM X data types. This
attribute is used to identify the locale of the user who provided the data. This property
is optional, and when it is not provided, a processor MAY process the data as if it were
provided in the default locale of the processor. When this property is provided, it overrides
the value of the property supplied by any containing data elements.

The values of the attribute are language identifiers as defined by [IETF BCP 47](http://tools.ietf.org/html/bcp47),
_Tags for the Identification of Languages_; in addition, the empty string may be specified to
explicitly state a processor may process the data as if it were provided in the default locale
of the processor.

In order to prevent undue burden on producers and consumers of GEDCOM X data, not all data types
provide multiple values for properties that are used for user input. For example, the text of
a note is _not_ defined as a list of language-identified strings. However, some cases have been
identified where multi-valued input is needed for the benefit of exchanging genealogical data
across cultural boundaries. Such cases include the need to input multiple name forms and the need
to identify multiple titles for a source.

## 1.4 Compliance

An implementation of the GEDCOM X conceptual model is "non-compliant" if it fails to satisfy
one or more of the MUST or REQUIRED level requirements. An implementation that satisfies all of
the  MUST or REQUIRED and all of the SHOULD level requirements is said to be "unconditionally
compliant"; and implementation that satisfies all of the MUST level requirements but not all of the
SHOULD level requirements is said to be "conditionally compliant".

# 2. Top-Level Data Types

The data types in this section are designated as "top-level" data types because they define the
top-level units of genealogical data. A GEDCOM X data set is defined as a collection of instances
of top-level data types. The definitions of top-level data types stand separately
from the definitions of other top-level data types and the lifecycle of instances of these data
types is distinct from the lifecycle of instances of the other top-level data types.

<a id="person"/>

## 2.1 The "Person" Data Type

The `Person` data type defines a description of a person.

### identifier

The identifier for the `Person` data type is:

`http://gedcomx.org/v1/Person`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
identifiers | Identifiers for the person. | List of [`http://gedcomx.org/v1/Identifier`](#identifier-type). Order is preserved. | OPTIONAL.
extracted | Whether the person is to be constrained as an *extracted conclusion*. A person that is identified as extracted conclusion MAY be referred to as a "persona". | boolean | OPTIONAL. Default: `false`. Refer to [Extracted Conclusion Constraints](#4-extracted-conclusion-constraints).
living | Whether the person is considered living. | boolean | OPTIONAL.
gender | The conclusion about the gender of the person. | [`http://gedcomx.org/v1/Gender`](#gender) | OPTIONAL.
names | The conclusions about the names of the person. | List of [`http://gedcomx.org/v1/Name`](#name-conclusion). Order is preserved. | OPTIONAL.
facts | The conclusions about the facts of the life of the person. | List of [`http://gedcomx.org/v1/Fact`](#fact-conclusion). Order is preserved. | OPTIONAL.
evidence | References to persons that support this conclusion. | List of [`http://gedcomx.org/v1/EvidenceReference`](#evidence-reference). Order is preserved. | OPTIONAL.  If provided, each reference MUST resolve to an instance of [`http://gedcomx.org/v1/Person`](#21-the-person-data-type).
media | References to multimedia resources for this person, such as photos or videos. Media references are intended to provide additional context or illustration for the person and SHOULD NOT be considered as evidence for conclusions. Media references SHOULD be ordered by priority such that applications that wish to display a single media item (such as an image) MAY choose the first applicable media reference. | List of [`http://gedcomx.org/v1/SourceReference`](#source-reference). Order is preserved. | OPTIONAL. Note that the `SourceReference` is used for multimedia references and therefore MUST resolve to a `SourceDescription` of the resource, which in turn provides a reference to the resource itself.
attribution | The attribution of the person. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing data set (e.g. file) of the person is assumed.


<a id="relationship"/>

## 2.2 The "Relationship" Data Type

The `Relationship` data type describes the relationship between two persons.

### identifier

The identifier for the `Relationship` data type is:

`http://gedcomx.org/v1/Relationship`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type | Enumerated value identifying the type of the relationship. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify a relationship type, and use of a [known relationship type](#known-relationship-types) is RECOMMENDED.
extracted | Whether this relationship is to be constrained as an *extracted conclusion*. | boolean | OPTIONAL. Default: `false`. Refer to [Extracted Conclusion Constraints](#4-extracted-conclusion-constraints).
person1 | Reference to the first person in the relationship. | [URI](#uri) | REQUIRED. MUST resolve to an instance of [`http://gedcomx.org/v1/Person`](#person)
person2 | Reference to the second person in the relationship. | [URI](#uri) | REQUIRED. MUST resolve to an instance of [`http://gedcomx.org/v1/Person`](#person)
facts | The conclusions about the facts of the life of the relationship. | List of [`http://gedcomx.org/v1/Fact`](#fact-conclusion). Order is preserved. | OPTIONAL.
evidence | References to relationships that supports this conclusion. | List of [`http://gedcomx.org/v1/EvidenceReference`](#evidence-reference). Order is preserved. | OPTIONAL.  If provided, each reference MUST resolve to an instance of [`http://gedcomx.org/v1/Relationship`](#22-the-relationship-data-type).
identifiers | Identifiers for the relationship. | List of [`http://gedcomx.org/v1/Identifier`](#identifier-type). Order is preserved. | OPTIONAL.
attribution | The attribution of the relationship. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing data set (e.g. file) of the relationship is assumed.

Note: when a relationship type implies direction, the relationship is said to
be *from* person1 *to* person2. For example, in a parent-child relationship, the
`person1` property refers to the parent and the `person2` property refers to the
child.

<a id="known-relationship-types"/>

### 2.2.1 Known Relationship Types

The following relationship types are defined by GEDCOM X:

URI | description
----|-------------
`http://gedcomx.org/Couple`| A relationship of a pair of persons.
`http://gedcomx.org/ParentChild`| A relationship from a parent to a child.


<a id="source-description"/>

## 2.3 The "SourceDescription" Data Type

The `SourceDescription` data type defines a description of a source of genealogical information.

### identifier

The identifier for the "SourceDescription" data type is:

`http://gedcomx.org/v1/SourceDescription`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
id | An identifier for the data structure holding the source description data. The id is to be used as a "fragment identifier" as defined by [RFC 3986, Section 3.5](http://tools.ietf.org/html/rfc3986#section-3.5). As such, the constraints of the id are provided in the definition of the media type (e.g. XML, JSON) of the data structure. | string | OPTIONAL.
citations | The citations for this source. At least one citation MUST be provided. If more than one citation is provided, citations are assumed to be given in order of preference, with the most preferred citation in the first position in the list. | [`http://gedcomx.org/v1/SourceCitation`](#source-citation) | REQUIRED.
mediaType | A hint about the media type of the resource being described. | string | OPTIONAL. If provided, MUST be a valid MIME (media) type as specified by [RFC 4288](http://tools.ietf.org/html/rfc4288).
resourceType | Enumerated value identifying the type of resource being described. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify a resource type, and use of a [known resource type](#known-resource-types) is RECOMMENDED.
about | A uniform resource identifier (URI) for the resource being described. | [URI](#uri) | OPTIONAL.
mediator | A reference to the entity that mediates access to the described source. | [URI](#uri) | OPTIONAL. If provided, MUST resolve to an instance of [`http://gedcomx.org/v1/Agent`](#agent).
sources | A list of references to any sources from which this source is derived. | List of [`http://gedcomx.org/v1/SourceReference`](#source-reference) | OPTIONAL.
componentOf | A reference to the source that contains this source, i.e. its parent context. Used when the description of a source is not complete without the description of its parent (or containing) source. | [`http://gedcomx.org/v1/SourceReference`](#source-reference) | OPTIONAL.
titles | The display names for this source. If more than one title is provided, titles are assumed to be given in order of preference, with the most preferred title in the first position in the list. | List of [`http://gedcomx.org/TextValue`](#text-value) | OPTIONAL.
notes  | A list of notes about a source. | List of [`http://gedcomx.org/Note`](#note) | OPTIONAL.
attribution | The attribution of this source description. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing data set (e.g. file) of the source description is assumed.

<a id="known-resource-types"/>

### 2.3.1 Known Resource Types

The following resource types are identified by GEDCOM X:

URI | description
----|------------
`http://gedcomx.org/Collection` | A collection of genealogical resources. A collection may contain physical artifacts (such as a collection of books in a library), records (such as the 1940 U.S. Census), or digital artifacts (such as an online genealogical application).
`http://gedcomx.org/PhysicalArtifact` | A physical artifact, such as a book.
`http://gedcomx.org/DigitalArtifact` | A digital artifact, such as a digital image of a birth certificate or other record.
`http://gedcomx.org/Record` | A historical record, such as a census record or a vital record.


<a id="agent"/>

## 2.4 The "Agent" Data Type

The `Agent` data type defines a living entity, such as a genealogical researcher, user of software, or organization.

### identifier

The identifier for the `Agent` data type is:

`http://gedcomx.org/v1/Agent`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
id | An identifier for the data structure holding the agent data. The id is to be used as a "fragment identifier" as defined by [RFC 3986, Section 3.5](http://tools.ietf.org/html/rfc3986#section-3.5). As such, the constraints of the id are provided in the definition of the media type (e.g. XML, JSON) of the data structure. | string | OPTIONAL.
identifiers | Identifiers for the agent. When an identifier for an agent is also an identifier for a person, the data in the person describes the agent. | List of [`http://gedcomx.org/v1/Identifier`](#identifier-type). Order is preserved. | OPTIONAL.
names | The names of the person or organization. If more than one name is provided, names are assumed to be given in order of preference, with the most preferred name in the first position in the list. | List of [`http://gedcomx.org/TextValue`](#text-value) | OPTIONAL.
homepage | The homepage of the person or organization. | [URI](#uri) | OPTIONAL.
openid  | The [openid](http://openid.net/) of the person or organization. | [URI](#uri) | OPTIONAL.
accounts  | The online accounts of the person or organization. | List of [`http://gedcomx.org/v1/OnlineAccount`](#online-account). Order is preserved. | OPTIONAL.
emails  | The email addresses of the person or organization. | List of [URI](#uri) - MUST resolve to a valid e-mail address (e.g. "mailto:someone@gedcomx.org"). Order is preserved. | OPTIONAL.
phones  | The phones (voice, fax, mobile) of the person or organization. | List of [URI](#uri) - MUST resolve to a valid phone number (e.g. "tel:+1-201-555-0123"). Order is preserved. | OPTIONAL.
addresses  | The addresses of the person or organization. | List of [`http://gedcomx.org/v1/Address`](#address). Order is preserved. | OPTIONAL.


<a id="event"/>

## 2.5 The "Event" Data Type

The `Event` data type defines a description of a historical event.

### identifier

The identifier for the `Event` data type is:

`http://gedcomx.org/v1/Event`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type | Enumerated value identifying the type of the event. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify an event type, and use of a [known event type](#known-event-types) is RECOMMENDED.
extracted | Whether this event is to be constrained as an *extracted conclusion*. | boolean | OPTIONAL. Default: `false`. Refer to [Extracted Conclusion Constraints](#4-extracted-conclusion-constraints).
date | The date of the event. | [`http://gedcomx.org/v1/Date`](#conclusion-date) | OPTIONAL.
place | A reference to the place applicable to this event. | [`http://gedcomx.org/v1/PlaceReference`](#conclusion-place-reference) | OPTIONAL.
roles | The roles of the persons in the event. | List of [`http://gedcomx.org/v1/EventRole`](#conclusion-event-role). Order is preserved. | OPTIONAL.
evidence | References to events that supports this conclusion. | List of [`http://gedcomx.org/v1/EvidenceReference`](#evidence-reference). Order is preserved. | OPTIONAL.  If provided, each reference MUST resolve to an instance of [`http://gedcomx.org/v1/Event`](#25-the-event-data-type).
media | References to multimedia resources for this event, such as photos or videos. Media references are intended to provide additional context or illustration for the event and SHOULD NOT be considered as evidence for conclusions. Media references SHOULD be ordered by priority such that applications that wish to display a single media item (such as an image) MAY choose the first applicable media reference. | List of [`http://gedcomx.org/v1/SourceReference`](#source-reference) | OPTIONAL. Note that the `SourceReference` is used for multimedia references and therefore MUST resolve to a `SourceDescription` of the resource, which in turn provides a reference to the resource itself.
attribution | The attribution of the event. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing data set (e.g. file) of the event is assumed.

<a id="known-event-types"/>

### 2.5.1 Known Event Types

The following event types are defined by GEDCOM X:

URI | description
----|------------
`http://gedcomx.org/Adoption` | An adoption event.
`http://gedcomx.org/Birth` | A birth event.
`http://gedcomx.org/Burial` | A burial event.
`http://gedcomx.org/Census` | A census event.
`http://gedcomx.org/Christening` | A christening event *at birth*. Note: use `AdultChristening` for a christening event as an adult.
`http://gedcomx.org/Death` | A death event.
`http://gedcomx.org/Divorce` | A divorce event.
`http://gedcomx.org/Marriage` | A marriage event.

In addition to these elements, processors SHOULD support any other elements defined by the
[GEDCOM X Event Types](https://github.com/FamilySearch/gedcomx/blob/master/specifications/event-types-specification.md) specification.


<a id="document"/>

## 2.6 The "Document" Data Type

The `Document` data type defines the base conceptual model for genealogical conclusions that are managed as textual documents.

### identifier

The identifier for the `Document` data type is:

`http://gedcomx.org/v1/Document`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type | Enumerated value identifying the type of the document. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify a document type, and use of a [known document type](#known-document-types) is RECOMMENDED.
textType | The type of text in the `text` field. | string | OPTIONAL. If provided, the value MUST be a [valid document text type](#document-text-types).  If no value is provided, "plain" is assumed.
text | The text of the document. | string | REQUIRED.
attribution | The attribution of the document. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing data set (e.g. file) of the document is assumed.

<a id="known-document-types"/>

### 2.6.1 Known Document Types

The following document types are defined by GEDCOM X:

URI | description
----|-------------
`http://gedcomx.org/Abstract` | The document is an abstract of a record or document.
`http://gedcomx.org/Transcription` | The document is a transcription of a record or document.
`http://gedcomx.org/Translation` | The document is a translation of a record or document.
`http://gedcomx.org/Analysis` | The document is an analysis done by a researcher, often used as a genealogical proof statement.

<a id="document-text-types"/>

### 2.6.2 Document Text Types

The following document text types are supported:

#### plain

The "plain" text type identifies the type the document text to be plain text. "plain" is the default value of a document.

#### xhtml

The "xhtml" text type identifies XHTML documents complying with the 
[XHTML 1.0 W3C Recommendation](http://www.w3.org/TR/xhtml1/). In order to maximize compatibility and 
minimize security risks, support for the following subset of modules (defined by XHTML Modularization 
1.1 W3C Recommendation](http://www.w3.org/TR/xhtml-modularization/)) is REQUIRED:

* [Core Modules](http://www.w3.org/TR/xhtml-modularization/abstract_modules.html#sec_5.2.), including the Structure Module, Text Module, Hypertext Module, and List Module.
* [Text Extension Modules](http://www.w3.org/TR/xhtml-modularization/abstract_modules.html#s_text), including the Presentation Module, Edit Module, and Bi-directional Text Module.
* [Table Modules](http://www.w3.org/TR/xhtml-modularization/abstract_modules.html#sec_5.6.), including the Basic Tables Module, and Tables Module.
* [Base Module](http://www.w3.org/TR/xhtml-modularization/abstract_modules.html#s_basemodule)

Support for other XHTML modules is OPTIONAL. Parsers MAY ignore elements from optional modules.

<a id="conclusion-place"/>


## 2.7 The "PlaceDescription" Data Type

The `PlaceDescription` data type describes the details of a place in terms of its name
and possibly its type, time period, and/or a geospatial description -- functioning as a description
of a place as a snapshot in time. The `PlaceDescription` data type extends the `Conclusion` data type.

### identifier

The identifier for the `PlaceDescription` data type is:

`http://gedcomx.org/v1/PlaceDescription`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
extracted | Whether this place description is to be constrained as an *extracted conclusion*. | boolean | OPTIONAL. Default: `false`. Refer to [Extracted Conclusion Constraints](#4-extracted-conclusion-constraints).
names | A list of standardized (or normalized), fully-qualified (in terms of what is known of the applicable jurisdictional hierarchy) names for this place that are applicable to this description of this place. | List of [http://gedcomx.org/v1/TextValue](#text-value). Order is preserved. | REQUIRED. The list MUST contain at least one name.
type | An implementation-specific uniform resource identifier (URI) used to identify the type of a place (e.g., address, city, county, province, state, country, etc.). | [Enumerated Value](#enumerated-value) | OPTIONAL.  There is no current definition of a set of known place types.
temporalDescription | A description of the time period to which this place description is relevant. | [`http://gedcomx.org/v1/Date`](#conclusion-date) | OPTIONAL.
latitude | Degrees north or south of the Equator (0.0 degrees). | IEEE 754 binary64 value | OPTIONAL.  If provided, MUST provide `longitude` also.  Values range from −90.0 degrees (south) to 90.0 degrees (north).  It is assumed that all instances of `PlaceDescription` that share an identical `Primary` identifier will also have identical `latitude` values.
longitude | Angular distance in degrees, relative to the Prime Meridian. | IEEE 754 binary64 value | OPTIONAL.  If provided, MUST provide `latitude` also.  Values range from −180.0 degrees (west of the Meridian) to 180.0 degrees (east of the Meridian).  It is assumed that all instances of `PlaceDescription` that share an identical `Primary` identifier will also have identical `longitude` values.
spatialDescription | A reference to a geospatial description of this place. | [`URI`](#uri) | OPTIONAL. It is RECOMMENDED that this geospatial description resolve to a KML document.
evidence | References to place descriptions that supports this conclusion. | List of [`http://gedcomx.org/v1/EvidenceReference`](#evidence-reference). Order is preserved. | OPTIONAL.  If provided, each reference MUST resolve to an instance of [`http://gedcomx.org/v1/Person`](#27-the-placedescription-data-type).
identifiers | A list of known identifiers for this place description (e.g., place authority identifiers). Multiple descriptions of the same place MAY be correlated via the `http://gedcomx.org/Primary` identifier. | List of [`http://gedcomx.org/v1/Identifier`](#identifier-type). Order is preserved. | OPTIONAL.
media | References to multimedia resources for this place, such as photos or videos. Media references are intended to provide additional context or illustration for the place and SHOULD NOT be considered as evidence for conclusions. Media references SHOULD be ordered by priority such that applications that wish to display a media item (such as an image) MAY choose the first applicable media reference. | List of [`http://gedcomx.org/v1/SourceReference`](#source-reference) | OPTIONAL. Note that the `SourceReference` is used for multimedia references and therefore MUST resolve to a `SourceDescription` of the resource, which in turn provides a reference to the resource itself.
attribution | Attribution metadata for this place description. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing data set (e.g. file) of the place description is assumed.


# 3. Component-Level Data Types

The data types in this section are designated as "component-level" data types because they are
designed to be referenced and re-used by other data types, most notably the top-level data types.
The lifecycle of instances of these data types is generally tied to the lifecycle of instances of
a top-level data type.

<a id="identifier-type"/>


## 3.1 The "Identifier" Data Type

The `Identifier` data type defines the data structure used to supply an identifier of a 
genealogical resource in a specific data set.

The `Identifier` data type does NOT support extension properties (see [Extension Properties](#extension-properties)).

### identifier

The identifier for the "Identifier" data type is:

`http://gedcomx.org/v1/Identifier`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
value | The value of the identifier. | [URI](#uri) | REQUIRED.
type  | Enumerated value identifying the type of the identifier. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify an identifier type, and use of a [known identifier type](#known-identifier-types) is RECOMMENDED.

<a id="known-identifier-types"/>

### known identifier types

The following identifier types are defined by GEDCOM X.

URI | description
----|------------
`http://gedcomx.org/Primary` | The primary identifier for the resource.
`http://gedcomx.org/Deprecated` | An identifier that has been relegated, deprecated, or otherwise downgraded. This identifier is commonly used as the result of a merge when what was once a primary identifier for a person is no longer primary.
`http://gedcomx.org/Persistent` | An identifier that is considered to be a long-term persistent identifier. Applications that provide persistent identifiers are claiming that links to the resource using the identifier won't break.

### examples

* Person "12345" merges into Person "67890". Person "67890" assumes identifier "12345". Identifier "12345" is of type `http://gedcomx.org/Deprecated`
  because the merged person "12345" now uses identifier "67890".
* An online web application issues a persistent identifier of value `https://familysearch.org/pal:/12345` to a `Person` and the same identifier
  is used as the primary identifier for the `Person`. The list of identifiers for the `Person` contains two identifiers with value `https://familysearch.org/pal:/12345`,
  one of type `http://gedcomx.org/Primary` and one of type `http://gedcomx.org/Persistent`.


<a id="attribution"/>

## 3.2 The "Attribution" Data Type

The `Attribution` data type defines the data structure used to attribute _who_, _when_, and _why_ to
genealogical data. Data is attributed to the agent who made the _latest significant change_ to the nature
of the data being attributed. The definition of a "significant change" is outside the scope of this specification
and is left to the implementer of the application.

#### The Granularity of Attribution

The granularity of data that is attributed varies widely from application to application. Some highly collaborative applications
might take a fine-grained approach, tracking attribution at the level of names, facts, and source references. Single-user
applications might simply provide attribution for a large set of data, such as an entire data tree. GEDCOM X explicitly
defines attribution for top-level entities such as persons, relationships, and documents, but also recognizes attribution
as an extension property that can be applied at a finer level of granularity. (For more information about extension properties,
see Section 6). For all data types where attribution is explicitly recognized, it is an OPTIONAL property.

If data is not explicitly attributed, the attribution for the data is assumed to be the attribution for the containing
data. For example, if no attribution is provided for the name of a person (as an extension property of the name), then the
attribution for the name is assumed to be the attribution of the person that contains the name. If no attribution for the
person is provided, the attribution is assumed to be the attribution for the data set that contains the person.

### identifier

The identifier for the "Attribution" data type is:

`http://gedcomx.org/v1/Attribution`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
contributor | Reference to the agent to whom the attributed data is attributed. | [URI](#uri) | OPTIONAL. If provided, MUST resolve to an instance of [`http://gedcomx.org/v1/Agent`](#agent).
modified | Timestamp of when the attributed data was contributed. | timestamp | OPTIONAL.
changeMessage | A statement of why the attributed data is being provided by the contributor. | string | OPTIONAL.

### examples

todo:


<a id="note"/>

## 3.3 The "Note" Data Type

The `Note` data type defines a note that was contributed from genealogical research.

Notes are not intended to contain genealogical conclusions.  Notes are only associated with a single genealogical resource.

### identifier

The identifier for the "Note" data type is:

`http://gedcomx.org/v1/Note`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
lang | The locale identifier for the note. | [IETF BCP 47](http://tools.ietf.org/html/bcp47) locale tag | OPTIONAL. If not provided, the locale of the containing resource of the note is assumed.
subject | A subject or title for the note. | string | OPTIONAL.
text | The text of the note. | string | REQUIRED.
attribution | The attribution of this note. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing resource of the note is assumed.


<a id="text-value"/>

## 3.4 The "TextValue" Data Type

The `TextValue` data type defines a literal text value.

The `TextValue` data type does NOT support extension properties (see [Extension Properties](#extension-properties)).

### identifier

The identifier for the "TextValue" data type is:

`http://gedcomx.org/v1/TextValue`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
lang | The locale identifier for the value of the text. | [IETF BCP 47](http://tools.ietf.org/html/bcp47) locale tag | OPTIONAL. If not provided, the local of the containing resource (or containing data set) of the text value the data is assumed.
value | The literal value. | string | REQUIRED.


<a id="source-citation"/>

## 3.5 The "SourceCitation" Data Type

The `SourceCitation` data type defines a container for the metadata necessary to identify a source(s)
and from which bibliographic citation strings may be rendered.

### identifier

The identifier for the "SourceCitation" data type is:

`http://gedcomx.org/v1/SourceCitation`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
lang | The locale identifier for the citation. | [IETF BCP 47](http://tools.ietf.org/html/bcp47) locale tag | OPTIONAL. If not provided, the locale of the data set containing the citation is assumed.
value | A rendering of the full (working) citation as a string. | string | REQUIRED.


<a id="source-reference"/>

## 3.6 The "SourceReference" Data Type

The `SourceReference` data type defines a reference to a source.  For example a genealogical conclusion
or a derivative source is the referring object (i.e. the object holding the `SourceReference` instance),
that cites its supporting source(s) (i.e. the target source(s)) using an instance(s) of `SourceReference`.

### identifier

The identifier for the "SourceReference" data type is:

`http://gedcomx.org/v1/SourceReference`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
description  | Reference to a _description_ of the target source. | [URI](#uri) | REQUIRED. MUST resolve to an instance of [`http://gedcomx.org/v1/SourceDescription`](#source-description).
attribution | The attribution of this source reference. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing resource of the source reference is assumed.

### examples

todo:


<a id="evidence-reference"/>

## 3.7 The "EvidenceReference" Data Type

The `EvidenceReference` data type defines a reference to data that is being used as evidence to answer a research question.
For example, a genealogical conclusion (i.e., the object holding the `EvidenceReference` instance) can refer to content extracted
from a source (i.e., an ["extracted"](#4-extracted-conclusion-constraints) `Conclusion`) as *evidence* that supports the conclusion.

### identifier

The identifier for the "EvidenceReference" data type is:

`http://gedcomx.org/v1/EvidenceReference`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
resource  | Reference to data being used as _evidence_. | [URI](#uri) | REQUIRED. MUST resolve to an instance of conclusion [`http://gedcomx.org/v1/Conclusion`](#311-the-conclusion-data-type).
analysis  | Reference to a document containing analysis that supports the use of the referenced data as _evidence_. | [URI](#uri) | OPTIONAL. If provided, MUST resolve to an instance of [`http://gedcomx.org/v1/Document`](#26-the-document-data-type) of type `http://gedcomx.org/Analysis`.
attribution | The attribution of this source reference. | [`http://gedcomx.org/Attribution`](#attribution) | OPTIONAL. If not provided, the attribution of the containing resource of the source reference is assumed.

### examples

* An application allows a researcher to extract information from a single census record about a person, representing the
  information as a [persona](#41-persona) with an identifier "abcde". The researcher extracts additional information about the
  person from a birth certificate and the application assigns the resulting [persona](#41-persona) an identifier "fghij". As the
  researcher gathers and analyzes the information, he will create a (working) `Person` conclusion. When the researcher concludes
  that the person represented in "abcde" and in "fghij" are the same person, he will add two `EvidenceReference` instances to the
  working `Person`: one for "abcde" and one for "fghij".


<a id="online-account"/>

## 3.8 The "OnlineAccount" Data Type

The `OnlineAccount` data type defines a description of an account in an online web application.

### identifier

The identifier for the `OnlineAccount` data type is:

`http://gedcomx.org/v1/OnlineAccount`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
serviceHomepage  | The home page of the service. | [URI](#uri) | REQUIRED.
accountName | The name, label, or id associating the owner of the account with the account. | string | REQUIRED.

<a id="address"/>

## 3.9 The "Address" Data Type

The `Address` data type defines a street address of a person or organization.

### identifier

The identifier for the `Address` data type is:

`http://gedcomx.org/v1/Address`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
value | A string representation of the value. Used when the address isn't separated into its constituent parts. | string | OPTIONAL.
city | The city. | string | OPTIONAL.
country | The country. | string | OPTIONAL.
postalCode | The postal code. | string | OPTIONAL.
stateOrProvince | The state or province. | string | OPTIONAL.
street | The street. | string | OPTIONAL.
street2 | The street (second line). | string | OPTIONAL.
street3 | The street (third line). | string | OPTIONAL.
street4 | The street (fourth line). | string | OPTIONAL.
street5 | The street (fifth line). | string | OPTIONAL.
street6 | The street (sixth line). | string | OPTIONAL.


<a id="conclusion"/>

## 3.10 The "Conclusion" Data Type

The `Conclusion` data type defines the base conceptual model for basic genealogical conclusions.

### identifier

The identifier for the `Conclusion` data type is:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
id | An identifier for the data structure holding the conclusion data. The id is to be used as a "fragment identifier" as defined by [RFC 3986, Section 3.5](http://tools.ietf.org/html/rfc3986#section-3.5). As such, the constraints of the id are provided in the definition of the media type (e.g. XML, JSON) of the data structure. | string | OPTIONAL.
lang | The locale identifier for the conclusion. | [IETF BCP 47](http://tools.ietf.org/html/bcp47) locale tag | OPTIONAL. If not provided, the locale of the current user of the data is assumed.
confidence  | Reference to the confidence level of the conclusion. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify a confidence level, and use of a [known confidence level](#known-confidence-levels) is RECOMMENDED.
sources | The list of references to the sources of related to this conclusion. The sources of a conclusion MUST also be sources of the conclusion's containing entity (i.e. [`Person`](#person) or [`Relationship`](#relationship) ).| List of [`http://gedcomx.org/v1/SourceReference`](#source-reference). Order is preserved. | OPTIONAL.
notes  | A list of notes about a conclusion. | List of [`http://gedcomx.org/Note`](#note) | OPTIONAL.

<a id="known-confidence-levels"/>

### 3.10.1 Known Confidence Levels

The following confidence levels are defined by GEDCOM X.

URI | description
----|------------
`http://gedcomx.org/High`|The contributor has a high degree of confidence that the assertion is true.
`http://gedcomx.org/Medium`|The contributor has a medium degree of confidence that the assertion is true.
`http://gedcomx.org/Low`|The contributor has a low degree of confidence that the assertion is true.


<a id="gender-conclusion"/>

## 3.11 The "Gender" Data Type

The `Gender` data type defines a conclusion about the gender of a person.

### identifier

The identifier for the `Gender` data type is:

`http://gedcomx.org/v1/Gender`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type  | Enumerated value identifying the type of the gender. | [Enumerated Value](#enumerated-value) | REQUIRED. MUST identify a gender type, and use of a [known gender type](#known-gender-types) is RECOMMENDED.


<a id="known-gender-types"/>

### 3.11.1 Known Gender Types

The following gender types are defined by GEDCOM X:

URI | description
----|-------------
`http://gedcomx.org/Male`| Male gender.
`http://gedcomx.org/Female`| Female gender.
`http://gedcomx.org/Unknown`| Unknown gender.


<a id="name-conclusion"/>

## 3.12 The "Name" Data Type

The `Name` data type defines a conclusion about a name of a person.

A `Name` is intended to represent a single variant of a person's name.  This means that nicknames, spelling variations,
or other name variants (often distinguishable by a name type) should be modeled with separate instances of `Name`.

The name forms of a name contain representations of this name.  A `Name` MUST contain at least one name form, presumably
a representation of the name that is considered proper and well formed in the person's native, historical cultural context.
Other name forms MAY be included, which can be used to represent this name in contexts where the native name form is not
easily recognized and interpreted.  Alternate forms are more likely in situations where conclusions are being analyzed
across cultural context boundaries that have both language and writing script differences.

For example, a Korean name has a native Korean form, but can also have a Chinese form and a Roman/Latin form -- three
different name forms, but each representing the same name.

If more than one name form is provided, included name forms are assumed to be given in order of preference, with the
most preferred name form in the first position in the list.

### identifier

The identifier for the `Name` data type is:

`http://gedcomx.org/v1/Name`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type | Enumerated value identifying the type of the name. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify a name type, and use of a [known name type](#known-name-types) is RECOMMENDED.
preferred | Whether this name is preferred above the other `Name` conclusions of a person. | boolean | OPTIONAL.
date | The date of applicability of the name. | [`http://gedcomx.org/v1/Date`](#conclusion-date) | OPTIONAL.
nameForms | The name form(s) that best express this name, usually representations considered proper and well formed in the person's native, historical cultural context. All included name forms SHOULD be representations of the same name, and NOT variants of the name (e.g., nicknames, spelling variations). | List of [`http://gedcomx.org/v1/NameForm`](#name-form). Order is preserved. | REQUIRED. At least one name form MUST be provided.

### examples

Consider the following: a Russian person with the birth name "Александр" (rendered as "Alexander" in English and in a Latin script) that
also went by this name's common nickname, "Саша" (rendered as "Sasha" in English).

It is tempting to think that this situation should be modeled with one `Name` instance that has several alternate `NameForm`s.  The model is
__*not*__ intended to be used in this way. Instead, this person's names ought to be modeled such that the birth name and the nickname are modeled
as two separate `Name` instances: one instance for the birth name, and one for the nickname.  The `type` property MAY be provided to distinguish
the birth name from the nickname. Each `Name` instance MAY have two `NameForm` instances: one with the native form of the name and another with
the alternate form.  Using an informal pseudo code, it might look something like the following:

```
Name1.type=http://gedcomx.org/BirthName
Name1.preferred=true
Name1.nameForms[0].fullText=Александр
Name1.nameForms[1].fullText=Alexander

Name2.type=http://gedcomx.org/Nickname
Name2.preferred=false
Name2.nameForms[0].fullText=Саша
Name2.nameForms[1].fullText=Sasha
```
<a id="known-name-types"/>

### 3.12.1 Known Name Types

The following name types are defined by GEDCOM X:

URI | description
----|-------------
`http://gedcomx.org/BirthName` | Name given at birth.
`http://gedcomx.org/MarriedName` | Name accepted at marriage.
`http://gedcomx.org/AlsoKnownAs` | "Also known as" name.
`http://gedcomx.org/Nickname`| Nickname.
`http://gedcomx.org/AdoptiveName` | Name given at adoption.
`http://gedcomx.org/FormalName` | A formal name, usually given to distinguish it from a name more commonly used.
`http://gedcomx.org/ReligiousName` | A name given at a religious rite or ceremony.


<a id="fact-conclusion"/>

## 3.13 The "Fact" Data Type

The `Fact` data type defines a conclusion about a fact of the life of a person or
the nature of a relationship. The `Fact` data type extends the `Conclusion` data type.

### identifier

The identifier for the `Fact` data type is:

`http://gedcomx.org/v1/Fact`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type | Enumerated value identifying the type of the fact. | [Enumerated Value](#enumerated-value) | REQUIRED. MUST identify a fact type, and use of a [known fact type](#known-fact-types) is RECOMMENDED.
date | The date of applicability of the fact. | [`http://gedcomx.org/v1/Date`](#conclusion-date) | OPTIONAL.
place | A reference to the place applicable to this fact. | [`http://gedcomx.org/v1/PlaceReference`](#conclusion-place-reference) | OPTIONAL.
value | The original value of the fact as supplied by the contributor. | string | OPTIONAL.
qualifiers | Qualifiers to add additional details about the fact. | List of [http://gedcomx.org/v1/Qualifier](#qualifier) | OPTIONAL. If present, use of a [known fact qualifier](#known-fact-qualifier) is RECOMMENDED.

<a id="known-fact-types"/>

### 3.13.1 Known Fact Types

The following fact types are defined by GEDCOM X:

URI | description | scope
----|-------------|------
`http://gedcomx.org/Adoption`| A fact of a person's adoption. In the context of a parent-child relationship, it describes a fact of the adoption of a child by a parent. | person
`http://gedcomx.org/Birth`| A fact of a person's birth. | person
`http://gedcomx.org/Burial`| A fact of the burial of person's body after death. | person
`http://gedcomx.org/Christening`| A fact of a person's christening *at birth*. Note: use `AdultChristening` for the christening as an adult. | person
`http://gedcomx.org/Death`| A fact of the death of a person. | person
`http://gedcomx.org/Residence`| A fact of a person's residence. | person
`http://gedcomx.org/Divorce`| The fact of a divorce of a couple. | couple relationship
`http://gedcomx.org/Marriage`| The fact of a marriage. | couple relationship

In addition to these elements, processors SHOULD support any other elements defined by the
[GEDCOM X Fact Types](https://github.com/FamilySearch/gedcomx/blob/master/specifications/fact-types-specification.md) specification.


<a id="known-fact-qualifier"/>

### 3.13.2 Known Fact Qualifiers

The following fact qualifiers are defined by GEDCOM X:

name | value
-----|-------
`http://gedcomx.org/Age`| The age of a person at the event described by the fact.
`http://gedcomx.org/Cause`| The cause of the fact, such as the cause of death.
`http://gedcomx.org/Religion`| The religion associated with a religious event such as a baptism or excommunication.

<a id="conclusion-event-role"/>

## 3.14 The "EventRole" Data Type

The `EventRole` data type defines a role played in an event by a person.  The `EventRole` data type extends the `Conclusion` data type.

### identifier

The identifier for the `EventRole` data type is:

`http://gedcomx.org/v1/EventRole`

### extension

This data type extends the following data type:

`http://gedcomx.org/v1/Conclusion`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
person | Reference to the person playing the role in the event. | [`URI`](#uri) | REQUIRED. MUST resolve to an instance of [`http://gedcomx.org/v1/Person`](#person).
type | Reference to the role type. | [`URI`](#uri) | OPTIONAL. If provided, must be a role type, and use of a [known role types](#known-roles) is RECOMMENDED.
details | Details about the role of the person in the event. | string | OPTIONAL.

<a id="known-roles"/>

### 3.14.1 Known Role Types

The following role types are defined by GEDCOM X:

URI | description
----|------------
`http://gedcomx.org/Principal`| The person is the principal person of the event. The principal of a birth event is the person that was born.
`http://gedcomx.org/Participant`| A participant in the event.
`http://gedcomx.org/Official`| An person officiating the event.
`http://gedcomx.org/Witness`| A witness of the event.


<a id="conclusion-date"/>

## 3.15 The "Date" Data Type

The `Date` data type defines the value of a genealogical date.

### identifier

The identifier for the `Date` data type is:

`http://gedcomx.org/v1/Date`

### properties

name | description | data type
-----|-------------|----------
original | The original value of the date as supplied by the contributor. | string | OPTIONAL.
formal | The standardized [formal value](#formal-values) of the date, formatted per GEDCOM X Date Format specification. | [GEDCOM X Date](https://github.com/FamilySearch/gedcomx/blob/master/specifications/date-model-specification.md) | OPTIONAL.


<a id="conclusion-place-reference"/>

## 3.16 The "PlaceReference" Data Type

The `PlaceReference` data type defines a reference to a description of a place.

### identifier

The identifier for the `PlaceReference` data type is:

`http://gedcomx.org/v1/PlaceReference`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
original | The original place name text as supplied by the contributor. | string | OPTIONAL.
descriptionRef | A reference to a _description_ of this place. | [URI](#uri) | OPTIONAL. If provided, MUST resolve to a [PlaceDescription](#conclusion-place-description).


<a id="name-part"/>

## 3.17 The "NamePart" Data Type

The `NamePart` data type is used to model a portion of a full name, including the terms that make up that portion. Some name parts MAY have qualifiers
to provide additional semantic meaning to the name part (e.g., "given name" or "surname").

A name part value MAY contain more than one term from the full name, such as in the name part "John Fitzgerald" from the full name "John Fitzgerald Kennedy".  If multiple terms are
detailed in a single `NamePart`, these terms are separated using the name separator appropriate to the locale of the name form.

### identifier

The identifier for the `NamePart` data type is:

`http://gedcomx.org/v1/NamePart`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
type | Enumerated value identifying the type of the name part. | [Enumerated Value](#enumerated-value) | OPTIONAL. If provided, MUST identify a name part type, and use of a [known name part type](#known-name-part-types) is RECOMMENDED.
value | The term(s) from the name that make up this name part. | string | REQUIRED.
qualifiers | Qualifiers to add additional semantic meaning to the name part. | List of [http://gedcomx.org/v1/Qualifier](#qualifier) | OPTIONAL. If present, use of a name part qualifier defined by the [GEDCOM X Name Part Qualifiers](https://github.com/FamilySearch/gedcomx/blob/master/specifications/name-part-qualifiers-specification.md) specification is RECOMMENDED.

<a id="known-name-part-types"/>

### 3.17.1 Known Name Part Types

The following name part types are defined by GEDCOM X:

URI | description
----|-------------
`http://gedcomx.org/Prefix`| A name prefix.
`http://gedcomx.org/Suffix`| A name suffix.
`http://gedcomx.org/Given`| A given name.
`http://gedcomx.org/Surname`| A surname.


<a id="name-form"/>

## 3.18 The "NameForm" Data Type

The `NameForm` data type defines a representation of a name (a "name form") within a given cultural context,
such as a given language and script.

As names are captured (in records or in applications), the terms in the name are sometimes classified by type.
For example, a certificate of death might prompt for "given name(s)" and "surname". The `parts` list can be
used to represent the terms in the name that have been classified.

If both a full rendering of the name and a list of parts are provided, it NOT REQUIRED that every
term in the fully rendered name appear in the list of parts.

Name parts in the `parts` list are presumed to be ordered in the natural order they would be spoken in the given
cultural context.

If a full rendering of the name is not provided (i.e., the name has only been expressed in `part`s), a full
rendering of the name can be derived (sans punctuation) by concatenating, in order, each `NamePart.value` in the
`parts` list, separating each part with the name part delimiter appropriate for the given `locale`.

### identifier

The identifier for the `NameForm` data type is:

`http://gedcomx.org/v1/NameForm`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
lang | The locale identifier for the name form. | [IETF BCP 47](http://tools.ietf.org/html/bcp47) locale tag | OPTIONAL. If not provided, the locale of the current user of the data is assumed.
fullText | A full rendering of the name (or as much of the name as is known) with the terms in the name given in the natural order they would be spoken in the applicable cultural context. | string | OPTIONAL.
parts | The parts of the name form, ordered in the natural order they would be spoken in the given cultural context. | List of [`http://gedcomx.org/v1/NamePart`](#name-part). Order is preserved. | OPTIONAL.

### examples

Consider the following: the Russian name "Пётр Ильи́ч Чайко́вский" in the Cyrillic script, its Latin-scr
ipt equivalent "Pyotr Ilyich Tchaikovsky", and its anglicised equivalent "Peter Ilyich Tchaikovsky".
Using an informal pseudo code, these name forms might be modeled as follows:

```
NameForm1.locale=ru-Cyrl
NameForm1.fullText=Пётр Ильи́ч Чайко́вский
NameForm1.parts[0].type=http://gedcomx.org/Given
NameForm1.parts[0].value=Пётр
NameForm1.parts[0].qualifiers[0]=http://gedcomx.org/First
NameForm1.parts[1].type=http://gedcomx.org/Middle
NameForm1.parts[1].value=Ильи́ч
NameForm1.parts[1].qualifiers[0]=http://gedcomx.org/Middle
NameForm1.parts[2].type=http://gedcomx.org/Surname
NameForm1.parts[2].value=Чайко́вский

NameForm2.locale=ru-Latn
NameForm2.fullText=Pyotr Ilyich Tchaikovsky
NameForm2.parts[0].type=http://gedcomx.org/Given
NameForm2.parts[0].value=Pyotr
NameForm2.parts[0].qualifiers[0]=http://gedcomx.org/First
NameForm2.parts[1].type=http://gedcomx.org/Given
NameForm2.parts[1].value=Ilyich
NameForm2.parts[1].qualifiers[0]=http://gedcomx.org/Middle
NameForm2.parts[2].type=http://gedcomx.org/Surname
NameForm2.parts[2].value=Tchaikovsky

NameForm3.locale=en-Latn
NameForm3.fullText=Peter Ilyich Tchaikovsky
NameForm3.parts[0].type=http://gedcomx.org/Given
NameForm3.parts[0].value=Peter
NameForm3.parts[0].qualifiers[0]=http://gedcomx.org/First
NameForm3.parts[1].type=http://gedcomx.org/Given
NameForm3.parts[1].value=Ilyich
NameForm3.parts[1].qualifiers[0]=http://gedcomx.org/Middle
NameForm3.parts[2].type=http://gedcomx.org/Surname
NameForm3.parts[2].value=Tchaikovsky
```

<a id="qualifier"/>

## 3.19 The "Qualifier" Data Type

The `Qualifier` data type defines the data structure used to supply additional details, annotations, tags, or other qualifying data
to a specific data element.

The `Qualifier` data type does NOT support extension properties (see [Extension Properties](#extension-properties)).

### identifier

The identifier for the "Qualifier" data type is:

`http://gedcomx.org/v1/Qualifier`

### properties

name  | description | data type | constraints
------|-------------|-----------|------------
name | The name of the qualifier, used to determine the nature of the qualifier. | [Enumerated Value](#enumerated-value) | REQUIRED. It is RECOMMENDED that the qualifier name resolve to an element of a constrained vocabulary.
value | The value of the qualifier. The semantic meaning of the value is determined by the qualifier name. | string | OPTIONAL.

### examples

todo:


<a id="extracted-conclusion-constraints"/>

# 4. Extracted Conclusion Constraints

GEDCOM X provides a specific definition for the term "extracted conclusion" that is used to refer to a set of constraints
that MUST be applied to conclusion data that is identified as "extracted" using the `extracted` property.

When a conclusion is identified as "extracted", it means that the data is to be treated as having been extracted from a
single source or record.  An extracted conclusion is distinguished from other conclusion data by the notion that it is
intended to describe information contained in a single source, as opposed to what a researcher or system believes to be true
about the subject of the conclusion.  Applications MUST recognize the `extracted` property and SHOULD ensure that any modifications
to the extracted conclusion are aligned with the information that is provided by the specific (single) source, even if the source
provides information that may conflict with information in other sources.

Data in a conclusion that is identified as "extracted" MUST conform to the following constraints:

* The conclusion (including any data it contains) MUST NOT refer to more than one source description.
* All source references used by the conclusion MUST resolve to the same source description, although
  each reference MAY contain distinct qualifying information such as attribution.

## 4.1 Persona

GEDCOM X provides a specific definition for the term "persona" that is used to refer to an instance of
`http://gedcomx.org/v1/Person` that has been identified as an extracted conclusion.


# 5. Extensibility

## Extensions from Non-GEDCOM X Vocabularies

This specification provides the data types and properties that define GEDCOM X.
Other data types and vocabularies ("foreign vocabularies") can be used in
GEDCOM X data types.

## Extensions to the GEDCOM X Vocabulary

The GEDCOM X namespaces are reserved for future forward-compatible revisions
of GEDCOM X. Future versions of this specification could add new data types and
properties to the GEDCOM X vocabulary.  Software written to conform to this
version of the specification will not be able to process such extensions correctly
and, in fact, will not be able to distinguish such extensions from error.  For
the purposes of this discussion, unrecognized data types and properties from the
GEDCOM X vocabulary will be considered "foreign vocabularies".

## Processing Foreign Vocabularies

GEDCOM X Processors that encounter foreign vocabularies in a location that is
legal according to this specification MUST NOT stop processing or
signal an error.  It might be the case that the GEDCOM X Processor is
able to process the foreign vocabularies correctly and does so.  Otherwise,
such vocabularies are termed "unknown foreign vocabularies".

When unknown foreign vocabularies are encountered, GEDCOM X Processors MAY
bypass the foreign properties, type references, and textual content and
MUST NOT change their behavior as a result of the presence of the vocabulary.

<a id="extension-properties"/>

## Extension Properties

GEDCOM X allows properties of foreign vocabularies in any data type, except
where it is explicitly forbidden.

## Extension Types

GEDCOM X allows data types of foreign vocabularies to be referenced anywhere
data types are defined to be referenced. Examples of data type references include
name types, fact types, resource types, etc. GEDCOM X supplies its own set of
"known" data types and implementors are encouraged to use the list of known data
types as much as possible to preserve maximum compatibility.

## User-Defined Types

Some applications MAY allow data types to be provided. For example, a genealogy
application may provide a feature that allows a user to supply a "custom" fact
about the life of a person when the list of known fact types doesn't contain
the type of fact the user may want to add.

In the case where a user has supplied a title or description instead of selecting
a known data type, GEDCOM X recognizes the data URI scheme as defined by
[RFC 2397](http://tools.ietf.org/html/rfc2397).

todo: data uri example

# 6. Miscellaneous To Do

todo: supply details about how GEDCOM X defines its evidence model.
