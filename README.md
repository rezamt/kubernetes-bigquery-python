
Copyright (C) 2014 Google Inc.

# Example apps: Real-time data analysis using Kubernetes, Redis or PubSub, and BigQuery

This repository contains two related example [Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes) apps that show how to build a 'pipeline' to stream data into BigQuery.

The first app, in the `redis` subdirectory, uses [Redis](http://redis.io/).
Documentation for this example can be found on the Google Cloud Platform site:
https://cloud.google.com/solutions/real-time-analysis/kubernetes-redis-bigquery

The second app, in the `pubsub` directory, uses [Google Cloud PubSub](https://cloud.google.com/pubsub/docs) instead of Redis. Documentation for this example can be found on the Google Cloud Platform site:
https://cloud.google.com/solutions/real-time/kubernetes-pubsub-bigquery


Note: Make sure the Google Cloud Alpha and Beta commands are installed.

gcloud components list
gcloud components install kubectl
gcloud components install alpha
gcloud components install beta


```bash
➜  google gcloud components list

Your current Cloud SDK version is: 157.0.0
The latest available version is: 157.0.0

┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                  Components                                                 │
├───────────────┬──────────────────────────────────────────────────────┬──────────────────────────┬───────────┤
│     Status    │                         Name                         │            ID            │    Size   │
├───────────────┼──────────────────────────────────────────────────────┼──────────────────────────┼───────────┤
│ Not Installed │ Cloud Bigtable Command Line Tool                     │ cbt                      │   4.0 MiB │
│ Not Installed │ Cloud Bigtable Emulator                              │ bigtable                 │   3.3 MiB │
│ Not Installed │ Cloud Datalab Command Line Tool                      │ datalab                  │   < 1 MiB │
│ Not Installed │ Cloud Datastore Emulator                             │ cloud-datastore-emulator │  15.4 MiB │
│ Not Installed │ Cloud Datastore Emulator (Legacy)                    │ gcd-emulator             │  38.1 MiB │
│ Not Installed │ Cloud Pub/Sub Emulator                               │ pubsub-emulator          │  21.0 MiB │
│ Not Installed │ Emulator Reverse Proxy                               │ emulator-reverse-proxy   │  14.5 MiB │
│ Not Installed │ Google Container Registry's Docker credential helper │ docker-credential-gcr    │   2.3 MiB │
│ Not Installed │ gcloud Alpha Commands                                │ alpha                    │   < 1 MiB │
│ Not Installed │ gcloud Beta Commands                                 │ beta                     │   < 1 MiB │
│ Not Installed │ gcloud app Java Extensions                           │ app-engine-java          │ 132.2 MiB │
│ Not Installed │ gcloud app PHP Extensions (Mac OS X)                 │ app-engine-php-darwin    │  21.9 MiB │
│ Installed     │ App Engine Go Extensions                             │ app-engine-go            │  96.7 MiB │
│ Installed     │ BigQuery Command Line Tool                           │ bq                       │   < 1 MiB │
│ Installed     │ Cloud SDK Core Libraries                             │ core                     │   6.1 MiB │
│ Installed     │ Cloud Storage Command Line Tool                      │ gsutil                   │   2.9 MiB │
│ Installed     │ Default set of gcloud commands                       │ gcloud                   │           │
│ Installed     │ gcloud app Python Extensions                         │ app-engine-python        │   6.4 MiB │
│ Installed     │ kubectl                                              │ kubectl                  │  14.8 MiB │
└───────────────┴──────────────────────────────────────────────────────┴──────────────────────────┴───────────┘
To install or remove components at your current SDK version [157.0.0], run:
  $ gcloud components install COMPONENT_ID
  $ gcloud components remove COMPONENT_ID

To update your SDK installation to the latest version [157.0.0], run:
  $ gcloud components update

➜  google gcloud components install alpha


Your current Cloud SDK version is: 157.0.0
Installing components from version: 157.0.0

┌──────────────────────────────────────────────┐
│     These components will be installed.      │
├───────────────────────┬────────────┬─────────┤
│          Name         │  Version   │   Size  │
├───────────────────────┼────────────┼─────────┤
│ gcloud Alpha Commands │ 2017.03.24 │ < 1 MiB │
└───────────────────────┴────────────┴─────────┘

For the latest full release notes, please visit:
  https://cloud.google.com/sdk/release_notes

Do you want to continue (Y/n)?  Y

╔════════════════════════════════════════════════════════════╗
╠═ Creating update staging area                             ═╣
╠════════════════════════════════════════════════════════════╣
╠═ Installing: gcloud Alpha Commands                        ═╣
╠════════════════════════════════════════════════════════════╣
╠═ Creating backup and activating new installation          ═╣
╚════════════════════════════════════════════════════════════╝

Performing post processing steps...done.

Update done!

➜  google gcloud components install beta


Your current Cloud SDK version is: 157.0.0
Installing components from version: 157.0.0

┌─────────────────────────────────────────────┐
│     These components will be installed.     │
├──────────────────────┬────────────┬─────────┤
│         Name         │  Version   │   Size  │
├──────────────────────┼────────────┼─────────┤
│ gcloud Beta Commands │ 2017.03.24 │ < 1 MiB │
└──────────────────────┴────────────┴─────────┘

For the latest full release notes, please visit:
  https://cloud.google.com/sdk/release_notes

Do you want to continue (Y/n)?  Y

╔════════════════════════════════════════════════════════════╗
╠═ Creating update staging area                             ═╣
╠════════════════════════════════════════════════════════════╣
╠═ Installing: gcloud Beta Commands                         ═╣
╠════════════════════════════════════════════════════════════╣
╠═ Creating backup and activating new installation          ═╣
╚════════════════════════════════════════════════════════════╝

Performing post processing steps...done.

Update done!

➜  google gcloud components list

Your current Cloud SDK version is: 157.0.0
The latest available version is: 157.0.0

┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                  Components                                                 │
├───────────────┬──────────────────────────────────────────────────────┬──────────────────────────┬───────────┤
│     Status    │                         Name                         │            ID            │    Size   │
├───────────────┼──────────────────────────────────────────────────────┼──────────────────────────┼───────────┤
│ Not Installed │ Cloud Bigtable Command Line Tool                     │ cbt                      │   4.0 MiB │
│ Not Installed │ Cloud Bigtable Emulator                              │ bigtable                 │   3.3 MiB │
│ Not Installed │ Cloud Datalab Command Line Tool                      │ datalab                  │   < 1 MiB │
│ Not Installed │ Cloud Datastore Emulator                             │ cloud-datastore-emulator │  15.4 MiB │
│ Not Installed │ Cloud Datastore Emulator (Legacy)                    │ gcd-emulator             │  38.1 MiB │
│ Not Installed │ Cloud Pub/Sub Emulator                               │ pubsub-emulator          │  21.0 MiB │
│ Not Installed │ Emulator Reverse Proxy                               │ emulator-reverse-proxy   │  14.5 MiB │
│ Not Installed │ Google Container Registry's Docker credential helper │ docker-credential-gcr    │   2.3 MiB │
│ Not Installed │ gcloud app Java Extensions                           │ app-engine-java          │ 132.2 MiB │
│ Not Installed │ gcloud app PHP Extensions (Mac OS X)                 │ app-engine-php-darwin    │  21.9 MiB │
│ Installed     │ App Engine Go Extensions                             │ app-engine-go            │  96.7 MiB │
│ Installed     │ BigQuery Command Line Tool                           │ bq                       │   < 1 MiB │
│ Installed     │ Cloud SDK Core Libraries                             │ core                     │   6.1 MiB │
│ Installed     │ Cloud Storage Command Line Tool                      │ gsutil                   │   2.9 MiB │
│ Installed     │ Default set of gcloud commands                       │ gcloud                   │           │
│ Installed     │ gcloud Alpha Commands                                │ alpha                    │   < 1 MiB │
│ Installed     │ gcloud Beta Commands                                 │ beta                     │   < 1 MiB │
│ Installed     │ gcloud app Python Extensions                         │ app-engine-python        │   6.4 MiB │
│ Installed     │ kubectl                                              │ kubectl                  │  14.8 MiB │
└───────────────┴──────────────────────────────────────────────────────┴──────────────────────────┴───────────┘
To install or remove components at your current SDK version [157.0.0], run:
  $ gcloud components install COMPONENT_ID
  $ gcloud components remove COMPONENT_ID

To update your SDK installation to the latest version [157.0.0], run:
  $ gcloud components update

```
