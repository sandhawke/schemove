status: not yet implemented, sorry

# schemove

_schemove_ is a tool for working with [movable RDF
schemas](https://sandhawke.github.io/mov/).  It has a command-line
mode for transforming data to support movable schemas and a library to
do the transformations inside your application.

## Command Line

By default, `schemove` maintains a current application schema for you in `schema.ttl` (in the current directory) and puts all imported data into files named `schemoved/$HOST/$PATH`.

```console
$ schemove 
```

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
$ schemove https://www.w3.org/People/Berners-Lee/card#i
```

```shell
$ schemove https://www.w3.org/People/Berners-Lee/card#i
```

## API

TBD

Something like scm.rename(quadstore, myschema)

where myschema might even default to something like require('../../schema.ttl'), so it's naturally in your app directory. Dunno how to really do that in nodejs.

## Formats

RDF is read using (TBD), so currently the acceptable formats are (X, Y, Z).  Data is output in the same format read, unless (TBD).  Inputs may be URLs or filenames.

