# Specification

### For the rules of the [Prez Profile of the Profiles Vocabulary (PPP)](https://w3id.org/profile/ppp)

## Metadata

**Property** | **Value**
--- | ---
IRI | `https://w3id.org/profile/ppp/spec`
Title | Specification for the Prez Profile of the Profiles Vocabulary
Description | Specifies the rules/requirements of the Prez Profile of the Profiles Vocabulary.
Created | 2022-09-06
Modified | 2022-09-06
Issued | 2022-09-06
Creator | [Nicholas J. Car](https://orcid.org/0000-0002-8742-7730)
Publisher | [Nicholas J. Car](https://orcid.org/0000-0002-8742-7730)
Version IRI | `https://w3id.org/profile/ppp/spec/1.0.0`

### Introduction

This specification document lists and describes the requirement of data to conform to the Prez Profile of the Profiles Vocabulary. The section [Requirements](#Requirements) below provides this.

This document does not provide machine-executable data validation rules: they are supplied by the [PPP Validator](https://w3id.org/profile/ppp/validator).

The document that lists all parts of the Prez Profile of the Profiles Vocabulary is the [PPP Profile Definition](https://w3id.org/profile/ppp).

#### Namespaces

The following prefixed namespaces are used throughout this document:

**Prefix** | **Namespace**
--- | ---
`dcterms`: `http://purl.org/dc/terms/`
`geo:` | `http://www.opengis.net/ont/geosparql#`
`ppp:` | `https://w3id.org/profile/ppp/`
`prof:` | `http://www.w3.org/ns/dx/prof/`
`rdfs:` | `http://www.w3.org/2000/01/rdf-schema#`
`sh:` | `http://www.w3.org/ns/shacl#`
`xsd:` | `http://www.w3.org/2001/XMLSchema#`

## Requirements

**ID** | **Title** | **Definition**
--- | --- | ---
R1 | Profile for RDF | Each `prof:Profile` must be formulated so it can be used with RDF data
R2 | Profile basic metadata | Each `prof:Profile` must present basic metadata of exactly one Title, Description, Creator, Publisher, Created, & Modified values indicated with `dcterms:title`, `dcterms:description`, `dcterms:creator`, `dcterms:publisher`, `dcterms:created` & `dcterms:modified` properties.
R3 | Constrained Classes | Each `prof:Profile` must indicate the OWL Classes it constrains, from the Standards the Profile profiles, with the `ppp:constrains` property, e.g. `<Profile-X> xxx:constrains geo:FeatureCollection , geo:Feature .`
R4 | Data Extraction Queries | For each class constrained by the profile, a Profile must supply a data extraction query template which is a SPARQL query template into which the IRI of an instance of the constrained class may be inserted, that will extract data conformant with the Profile from a database

## Examples

### Valid Profile

This is a valid `prof:Profile` instances:

```
<http://www.opengis.net/def/geosparql>
    a prof:Profile ;
    dcterms:description "An RDF/OWL vocabulary for representing spatial information" ;
    dcterms:identifier "geo" ;
    dcterms:title "GeoSPARQL" ;
    ppp:constrains
        geo:Feature ,
        geo:FeatureCollection ;
    prof:hasResource [
        prof:role ppp:dataExtraction ;
        sh:targetClass geo:Feature ;
        prof:hasArtifact "http://example.com/some/query-template.sparql"^^xsd:anyURI ;
    ] ;
.
```

### Query Templates

This is a [SPARQL query](https://www.w3.org/TR/sparql11-query/) template for [GeoSPARQL](https://opengeospatial.github.io/ogc-geosparql/geosparql11/spec.html)'s `Feature` class. Note that there is not just _one_ such possible query template but many since different queries may be used to extract different data for a class, each of which may be valid according to a particular profile.

```
SELECT *
WHERE {
    ?f a geo:Feature ;
        rdfs:label ?label ;
        dcterms:identifier ?id ;
        go:hasGeometry [
            geo:asWKT ?wkt ;
        ] ;
    .
    
    OPTIONAL {
        ?f rdfs:comment ?comment .
    }
}
```

