Reproduction notes for https://github.com/grpc/grpc-node/issues/2502

Currently this repo is only being used as a way to share large trace log files.
It is not full reproduction code.

Observed behavior: Long running k8s pods in a GKE cluster in us-central1 will,
on occasion, get into a stuck state where every PubSub publish will result in a
DEADLINE_EXCEEDED error. This state persists with an increasing memory usage
until the pod is killed, either manually or from OOM.

The trace logs were generated with a small forever loop which publishes a
pubsub message every 500 milliseconds. Retry settings are the [defaults from
pubsub-nodejs](https://github.com/googleapis/nodejs-pubsub/blob/main/src/v1/publisher_client_config.json#L29-L37)
but with `initialRpcTimeoutMillis` set to 5000ms.
