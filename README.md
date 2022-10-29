# tsdb-snapshot-upload

A simple binary that uploads a TSDB Snapshot to S3.

## summary

The binary re-uses the synchronous logic from the thanos project for the pkg/shipper, which is responsible for performing the upload of a TSDB block.

This code aims to update a TSDB snapshot (i.e. meta.json) so that the underlying blocks can be synced to an object store. 

# use-case

Downscaling `Prometheus` shards via Prometheus Operator. When we reduce the `spec.shards`, we can take a TSDB Snapshot using Prometheus HTTP Admin API so that we save the memory/wal into a format that is (almost) ready for uploading to an object store.

Thanos Sidecar will normally take care of this, but it only uploads blocks every 2 hrs. This means on downscale, we risk the loss of 2 hrs of metrics.

My solution to this is to take a TSDB snapshot and then use the same logic from thanos project for updating the underlying snapshot blocks to upload to an object store. 
