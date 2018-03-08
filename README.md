# pretzel-data

Data structure resources for plantinformatics/pretzel.

## High level structure

Data is organised into *datasets* which contain *blocks*. Think of a *dataset* as a container for a
collection of data which can be naturally organised into subsets which form *blocks*. Blocks must
have *scope* which differentiates them from other blocks.

For example, we can define a simple physical genome as follows:

```
{
  "name": "myGenome",
  "meta": {
    "year": "2018"
  },
  "blocks": [
    {
      "scope": "1A",
      "featureType": "linear",
      "range": [
        1,
        500000000
      ]
    },
    {
      "scope": "1B",
      "featureType": "linear",
      "range": [
        1,
        450000000
      ]
    }
  ]
}
```

`myGenome` has two chromosomes (which correspond to *blocks* here), `1A` and `1B`. `featureType`
indicates the type of feature contained in the block. Presently, all data is `linear`, which defines
a range and features as positions or sub-ranges within the range. The plan is for future data such
as genotypes to be of `observational` type.

The `meta` field contains a set of arbitrary key-value pairs. It can be empty. It can be used to
record any associated metadata, such as publication DOI, year, source, details of the organism,
variety, etc.

The above dataset defines a physical genome with two chromosomes of given sizes. This is like
defining the "reference" in a genome viewer tool. Next, we want to define an annotation inside this
space.

## Defining features inside a dataset

We define features, such as genes, inside another dataset by using the field `parent`:

```
{
    "name": "myAnnotation",
    "parent": "myGenome",
    "namespace" : "myGenome:myAnnotation",
    "blocks": [
        {
            "scope": "1A",
            "featureType": "linear",
            "features": [
                {
                    "name": "my1AGene1",
                    "range": [
                        3000,
                        5150
                    ]
                },
                ...
```

By specifying the `parent` as `myGenome` (defined previously), we are indicating that the `scope` we
reference in the following dataset is refering to the parent blocks. Here we have defined a gene
`my1AGene1` spanning positions 3000 to 5150 in chromosome (`scope`) `1A`. Negative orientation of a
gene can be defined by having the second value in the range smaller than the first.

