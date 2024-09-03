# Butler Databases for Community Science Users at the RSP

```{abstract}
Describes the database infrastructure that supports end-user access to Butler metadata via the Rubin Science Platform.
```

## Scope


Not included in this document are:
* Butler databases used internally by the project team
* Butler databases hosted by other institutions (which may contain some of the same data as the databases described here.)
* The filesystem containing the image data ("Datastore" in Butler jargon)

## Taxonomy of Butler databases

### Database purpose
#### Registry
#### ObsCore

### Update modes
#### Static
#### Batch update
#### Online user-generated

### Schema ("Dimension Universe")

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