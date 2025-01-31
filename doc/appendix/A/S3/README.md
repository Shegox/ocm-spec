# Proposal: AWS S3 as Backend for the Open Component Model

This specification describes how an OCM repository view is mapped
to an S3 blob store.

## Specification Format

To describe a [repository context](../../../specification/elements/README.md#repository-contexts)
for an OCM repository conforming to this specification the following
repository specification format MUST be used.

### Synopsis

```
type: S3/v1
```

### Description

Artefact namespaces/repositories of the API layer will be mapped to an 
S3 bucket.

### Specification Versions

Supported specification version is `v1`.

#### Version `v1`

The type specific specification fields are:

- **`bucketURL`** *string*

  S3 bucket reference

## Element Mapping

An *OCM repository* is mapped to an *S3 bucket*.

The component id is mapped to an object path below this bucket, followed
by a namespace component `__versions__`.

The OCM *component version* is stored below an additional folder with the
version name of the component version.

All artefacts belonging to a component version are stored as blobs below
this folder.

The OCM *component descriptor* of a component version is stored with the
name `component-descriptor.yaml` in [YAML](https://yaml.org/spec/) format.

Local blobs are stored as additional blobs in the same folder. The
blob identity is the name of the blob object in this folder.

The name should be derived from the digest of the blob.

For example:

```
└── bucket
    ├── <componentid>
    │   └── __versions__
    │       └── <version>
    │           ├── component-descriptor.yaml
    │           ├── <sha256-1>
    │           └── <sha256-2>
    └── github.com
        └── gardener
            └── external-dns-management
                └── __versions__
                    └── 1.0.0
                        ├── component-descriptor.yaml
                        └── sha256.1d4382e73dc767efc4f3cf43cb970d09104ea26301fc1495244c11e2ad45639e

```

## Blob Mapping

Because the base repository is a pure blob store, no dedicated
blob mappings are required.