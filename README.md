[![CodeFactor](https://www.codefactor.io/repository/github/rhzs/serverless-ktp-ocr/badge/master)](https://www.codefactor.io/repository/github/rhzs/serverless-ktp-ocr/overview/master)

## Serverless Indonesian KTP OCR with GCP

This is reproducible work from [Ekstraksi Informasi e-KTP dengan Google Cloud Function dan Cloud Vision API](https://medium.com/@imrenagi/ekstraksi-informasi-e-ktp-dengan-google-cloud-function-dan-cloud-vision-api-4655db21d084). It also contains code fix to match with ES6 compliant module usages.

### KTP OCR Architecture

![Alt text](docs/arch.png?raw=true "E-KTP OCR with GCP")

### Pre-requisites

1. Google Cloud Account
2. `gcloud` CLI
3. Create New Google Cloud project `gcloud projects create ektp-ocr`
4. Enable Google Cloud Functions `gcloud services enable cloudfunctions.googleapis.com`
5. Enable Google Cloud Storage `gcloud services enable storage-api.googleapis.com` `gcloud services enable storage-component.googleapis.com`, create bucket name `uploaded_ktp`
6. Enable Google Cloud PubSub `gcloud services enable pubsub.googleapis.com`, create topic name `ektp-text-extracted`
7. Enable Google Cloud Vision [this link](console.cloud.google.com/flows/enableapi?apiid=vision.googleapis.com)

### How to deploy

1. Deploy HTTP

```shell
$ cd http-ktp
$ npm i
$ gcloud functions deploy http-ktp --entry-point uploadKtp --trigger-http --runtime=python37
```

2. Deploy KTP Image Event Trigger

```shell
$ cd ktp-iamge-event-trigger
$ npm i
$ gcloud functions deploy processImageFromGCSEvent --trigger-resource uploaded_ektp --trigger-event google.storage.object.finalize
```

3. Deploy Extraction Data

```shell
$ gcloud functions deploy extract_ktp --runtime python37 --trigger-topic ektp-text-extracted
```

### Results

View Stackdriver logs from `extract-ktp` function.

### Love it?

Please support me by **stars* this Github repo. Thank you!
