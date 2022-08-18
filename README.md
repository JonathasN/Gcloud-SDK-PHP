# Gcloud SDK + PHP
## Usage

To use this image, run the following command:

```
docker pull jonathasn/gcloud-sdk-php:5.6-cli-jessie
```

Verify the install

```bash
docker run -it jonathasn/gcloud-sdk-php:5.6-cli-jessie gcloud version
Google Cloud SDK 299.0.0
```

You can authenticate `gcloud` with your user credentials by running [`gcloud auth login`](https://cloud.google.com/sdk/gcloud/reference/auth/login):

```
docker run -ti --name gcloud-config jonathasn/gcloud-sdk-php:5.6-cli-jessie gcloud auth login
```

If you need to authenticate any program that uses the Google Cloud APIs, you need to pass the `--update-adc` option:

```
docker run -ti --name gcloud-config jonathasn/gcloud-sdk-php:5.6-cli-jessie gcloud auth login --update-adc
```

If you want to use a specific project for future uses, you can set this inside the `gcloud-config` container:

```
docker run -ti --name gcloud-config jonathasn/gcloud-sdk-php:5.6-cli-jessie /bin/bash -c 'gcloud auth login && gcloud config set project your-project'
```

Once you authenticate successfully, credentials are preserved in the volume of
the `gcloud-config` container.

To list compute instances using these credentials, run the container with
`--volumes-from`:

```
docker run --rm --volumes-from gcloud-config jonathasn/gcloud-sdk-php:5.6-cli-jessie gcloud compute instances list --project your_project
NAME        ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP      STATUS
instance-1  us-central1-a  n1-standard-1               10.240.0.2   8.34.219.29      RUNNING
```

> :warning: **Warning:** The `gcloud-config` container now has a volume
> containing your Google Cloud credentials. Do not use `gcloud-config` volume in
> other containers.
