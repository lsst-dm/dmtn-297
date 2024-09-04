# Butler Databases for Community Science Users at the RSP

```{abstract}
Describes the database infrastructure that supports end-user access to Butler metadata via the Rubin Science Platform.
```

## Scope


Not included in this document are:
* Butler databases used internally by the project team
* Butler databases hosted by other institutions (which may contain the same data as the databases described here.)
* The filesystem containing the image data ("datastore" in Butler jargon)

## Taxonomy of Butler databases

### Database access method

There are two primary methods of accessing Butler metadata, shown in the table
below.  These two access methods use two different database schemas.

| Access Method | Schema |
| ------------- | ------ |
| Butler library | Butler Registry |
| IVOA TAP service | ObsCore |

Confusingly, these two schemas sometimes co-exist in the same database and
sometimes are deployed separately.  Some considerations for how these databases
are configured for each deployment are described below.


#### Registry (supports Butler library)

- postgres and sqlite
- expect to explore read replicas or AlloyDB for scalability

#### ObsCore (supports TAP)

ObsCore is an [IVOA standard](https://www.ivoa.net/documents/ObsCore/) defining
a set of observation metadata, and a protocol for accessing it.  The metadata
for some Butler dataset types in the Registry can be presented as an ObsCore
table.

We currently have two ways to host ObsCore data derived from Butler metadata:
* Loaded into [QServ](https://dmtn-243.lsst.io/) alongside the LSST catalogs.
* Co-located in the same Postgres database as the Butler Registry, in a separate table.

In both of these cases, end users access the data via a [TAP
service](https://github.com/lsst-sqre/lsst-tap-service), [hosted in the Rubin
Science Platform](https://phalanx.lsst.io/applications/tap/index.html).

##### QServ

ObsCore via QServ is used for the annual data releases, because QServ provides
a scalable cluster of database instances.  When the data release is made, [a
script](https://github.com/lsst-dm/dax_obscore) is used to convert metadata
from the Butler Registry DB into a format that can be ingested by QServ.

QServ is not well-suited for dynamically updated data, so this setup is only
used for static datasets.

##### Co-located with Registry DB

The ObsCore table can also be hosted in Postgres, sharing a database with the
Butler Registry.  The Butler library can be configured with an ["ObsCore
Manager"](https://github.com/lsst/daf_butler/blob/50c0a9ef673d2b360f76026ea5d13829836f77b2/python/lsst/daf/butler/registry/obscore/_manager.py#L115)
that maintains an ObsCore table containing information about datasets in the
Registry.  As datasets are added through the usual Butler methods for updating
the Registry, the ObsCore table is updated simultaneously.

For spatial indexing, the ObsCore table uses the [pg_sphere Postgres
extension](https://github.com/postgrespro/pgsphere).  This extension is fairly
niche, with few use cases outside of astronomy.  It is not supported by Google
Cloud SQL, making deployment at the [Rubin Science Platform on Google
Cloud](https://dmtn-209.lsst.io/) more difficult.


### Update modes
#### Static
#### Batch update
#### Online user-generated

### Metadata Schema ("Dimension Universe")

## Butler database deployments

(architecture diagram)


### Data Preview 0.2
#### Summary
* Status: Deployed
* Registry: Google Cloud SQL at IDF
* ObsCore: Separate database (QServ)
* Update mode: Static and online user-generated

### Data Preview 1 and Data Releases
#### Summary
* Status: Planned for 2025
* Registry: Google Cloud SQL at IDF
* ObsCore: Separate database (QServ)
* Update mode: Static

### Prompt data products
#### Summary
* Status: Planned for 2025
* Registry: Postgres at USDF
* ObsCore: In same Postgres as registry at USDF
* Update mode: Batch (updated daily)

### User-generated data
#### Summary
* Status: Part of a system that is not yet designed
* Registry: Google Cloud SQL at IDF
* ObsCore: N/A
* Update mode: Online user-generated