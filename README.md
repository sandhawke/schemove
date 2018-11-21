status: not yet implemented, sorry

# schemove

_schemove_ is a tool for working with movable RDF schemas.  It has a
command-line mode for transforming data to support movable schemas and a
library to do the transformations inside your application, so it can work
with movable schemas that have moved.

## Why Movable Schemas?

Because:

1. You don't have quite enough faith in the people hosting a schema you want to use. You're concerned it might go offline or be changed in some way that causes problems.
2. You want to offer a schema without asking its users to have faith in you maintaining it, safely, forever
3. You don't want to have to maintain it, safely, forever
4. You want your data and software to automatically handle schemas changing over time

Sounds good?

Of course it's not all perfect:


* You don't get benefits 2 and 3 until you can assume everyone using your schema is using this software (or something compatible).  For now, it's best to offer stable hosting of your schema if you can, and push people to use movable schema software to reduce your responsibility.
* Benefits 1 and 4 require people to make their schemas movable.

TBD point to a site where we can find and share and create movable schemas.

## What Do I Have To Do?

### Reading RDF

### Write Data

### Creating a Movable Schema

### Converting an Existing Schema

## What Is A Movable Schema?

An RDF schema, sometimes called an "ontology" or "vocabulary", is an RDF
graph which defines properties, classes, and individuals to be used in
other (data) graphs.  When data graphs use the same schemas, they are
generally interoperable.

A _movable_ schema is one which does not depend on the URIs of its
elements for their meaning.  Instead, it depends on the text of a
natural language definition.  (But don't worry, there's no AI
here. This is just simple string matching.  The meaning of the text
only matters to human readers.)

### Example

Imagine you need a "family name" property.

With a traditional non-movable schema, you could use <http://xmlns.com/foaf/0.1/familyName>.  That URL is accepted as denoting the family name property. It gets that meaning by common practice where the owner of xmlns.com is granted some freedom in specifying the meaning of terms hosted there, and the community tends to adopt that meaning. (See [Social Meaning of RDF](https://www.w3.org/wiki/SocialMeaning).)

With a movable schema, you can use whatever URL you like (include a fragment URL within your data file), and it still has the same semantics, as long as you include an appropriate definition, like:

```turtle
:familyName mov:propdef "The family name or surname. This is part of a person's name which is typically shared with members of their immediate family, in contrast with their given name." 
```

With a _non-movable_ schema, the machine doesn't care about the text of the URI -- it just looks for an exact match of that _URI string_.

With a _movable_ schema, the machine doesn't care about the text of the definition -- it just looks for an exact match of that _definition string_.

With movable schemas, two data graphs will be interoperable even if they use different URLs for familyName, as long as they use that exact same definition for it.

## Command Line

In its simplest mode, schemove reads an RDF data file or schema and checks whether all its schema terms are movable.  Any terms without a movable definition are reported as errors.

```console
$ schemove --check input.ttl
```

Certain domain can be treated as trustworthy enough that we don't need
their schemas to be movable.  You can declare them like this:


```console
$ schemove --except w3.org --except schema.org --check input.ttl
```

(The schemove machinery itself treats a few URIs as defined by fiat,
not subject to change or ever needing to move.  These are all URIs
defined in long-standing W3C Recommendations and hosted at w3.org,
such as rdf:type and owl:InverseFunctionalProperty.)

If a file passes "--check", you can run it in default mode which
gathers and includes all needed definitions.  The output will be
data which retains its meaning without the rest of the web.

```console
$ schemove input.ttl > output-including-schema.ttl
```

(We probably want several versions for what URIs to use. --keep, --fragment, --blank, --prefix=..., )

In practice, you may want the schema to be in a different file:

```console
$ schemove --local appendable-schema.ttl input.ttl > output.ttl
```

## API

TBD

Something like scm.rename(quadstore, myschema)

where myschema might even default to something like require('../../schema.ttl'), so it's naturally in your app directory. Dunno how to really do that in nodejs.

## Formats

RDF is read using (TBD), so currently the acceptable formats are (X, Y, Z).  Data is output in the same format read, unless (TBD).  Inputs may be URLs or filenames.

