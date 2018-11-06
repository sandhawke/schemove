
# schemove

_schemove_ is a tool for working with movable RDF schemas.  It has a
command-line mode for transforming data to support movable schemas and a
library to do the transformations inside your application, so it can work
with movable schemas that have moved.

## Why Movable Schemas?

Because:

1. The schema you want to use might go off-line at some point
2. The schema you want to use might be changed incompatibly, some day
3. You want to offer a schema without needing to keep a server up 24x7 forever
4. You want to offer a schema without needing that server to remain secure
5. You want to offer a schema without being well known and widely trusted
6. You want your code to automatically handle schema evolution

## What Is A Movable Schema?

An RDF schema, sometimes called an "ontology" or "vocabulary", is a RDF
graph which defines properties, classes, and individuals to be used in
other (data) graphs.  When data graphs use the same schemas, they are
generally interoperable.

A _movable_ schema is one which does not depend on the URIs of its
elements for their meaning.  Instead, it depends on a natural language
definition.

For example, imagine you need a "family name" property.

With a traditional non-movable schema, you could use <http://xmlns.com/foaf/0.1/familyName>.  That URL is accepted as denoting the family name property. It gets that meaning by common practice where the owner of xmlns.com is granted some freedom in specifying the meaning of terms hosted there, and the community tends to adopt that meaning. (See [Social Meaning of RDF](https://www.w3.org/wiki/SocialMeaning).)

With a movable schema, you can use whatever URL you like (include a fragment URL within your data file), and it still has the same semantics, as long as you include an appropriate definition, like:

```turtle
:familyName mov:propdef "The family name or surname. This is part of a person's name which is typically shared with members of their immediate family, in contrast with their given name." 
```

With a non-movable schema, the machine doesn't care about the text of the URI -- it just looks for an exact match of that URI in other places.

With a movable schema, the machine doesn't care about the text of the definition -- it just looks for an exact match of that text in other places.

With movable schemas, two data graphs will be intererable even if they use different URLs for familyName, as long as they use that exact same definition for it.

## Command Line

In its simplest mode, schemove reads an RDF data file or schema and
checks whether all its schema terms are movable.  Any which are not
are reported as errors.

```sh
schemove --check input.ttl
```

Certain domain can be treated as trustworthy enough that we don't need
their schemas to be movable.  You can let them by like this:


```sh
schemove --except w3.org --except schema.org --check input.ttl
```

(The schemove machinery itself treats a few URIs as defined by fiat,
not subject to change or ever needing to move.  These are all URIs
defined in long-standing W3C Recommendations and hosted at w3.org,
such as rdf:type and owl:InverseFunctionalProperty.)

If a file passes "--check", you can run it in default mode which
gathers an includes all needed definitions.  The output will be
data which retains its meaning without the rest of the web.

```sh
schemove input.ttl > output-including-schema.ttl
```

(We probably want several versions for what URIs to use. --keep, --fragment, --blank, --prefix=..., )

In practice, you may want the schema to be in a different file:

```sh
schemove --local appendable-schema.ttl input.ttl > output.ttl
```

## API

TBD
