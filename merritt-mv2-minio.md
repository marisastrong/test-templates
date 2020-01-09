# Swift to Minio
These are the steps need for phase 1 flip of migrating Merritt from using SDSC Swift content to Minio.

With this process:
* Dryad will remain fully functional
* During process no updates to Merritt will take place
## Steps
* Extension
* Shutdown all new changes swift
* Flip
* Restart
## Extension (hopeful)
### (Eric) See if we can get an extension to (preferred) end February (OK) mid-February

This will help get better confirmation things are working. Current hope is flip last week Jan.
## Shutdown all new changes swift
Requires that all swift changes have completed on system 
### (Mark) Pause ingest
This will need to be done maybe 2 hours before flip to guarantee that no non-Dryad processing is taking place.

_?Big question whether the queued profiles during freeze contain node information - if so soft shutdown may not be possible._
### (David) Confirmation that all ingest complete through replication
This require running tests to guarantee that all swift content has been replicated to minio
### (Mark) Shutdown monitors
* Monitors for replication
* Monitors for inventory
### (David?) Database checkpoint
This step needs to be confirmed with IAS
### (David) Shutdown replication
This is to guarantee no further caching of changes
## Flip
### (David) build new inv_nodes_inv_objects table.
* replace all primary swift with primary minio
* replace all secondary swift with secondary minio
* retain all non-swift
* add secondary for all swift (phase 1)
### (David) confirm alternate nodes_objects tables are accurate
### (David) build new inv_collections_inv_nodes table.
This:
* retains non-swift
* replaces as secondary swift with minio
* retains all swift as secondary to continue preserving content
### (David) confirm new inv_collections_inv_nodes table.
### (Mark) flips ingest profiles
## Restart
### (David) restart replication
### (Mark) server restarts on inventory
This is to guarantee no db caching. Also it will force inv to work with latest Minio content moving forward
### (Mark,David) _?test ingest flip_
Not clear if this can be done with frozen ingest
Includes:
* Mark - add new archive content (Minio)
* David - confirm inv updated inv_nodes_inv_objects
* David - confirm replication of minio content to glacier
* Mark - add new public content (AWS)
* David confirm inv updated
* David - confirm replication of AWS content to both minio
* David - confirm replication of AWS content to swift (phase 1)
### (Mark) unfreeze ingest
### (Mark) restore (or use timeout) monitors
