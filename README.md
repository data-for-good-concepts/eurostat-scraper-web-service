
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Eurostat Scraper <img src="man/figures/logo.png" align="right" height="139"/>

<!-- badges: start -->

[![Lifecycle:
experimental](https://lifecycle.r-lib.org/articles/figures/lifecycle-experimental.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
[![deploy-cloudrun](https://github.com/data-for-good-concepts/eurostat-scraper/actions/workflows/deploy-cloudrun.yaml/badge.svg)](https://github.com/data-for-good-concepts/eurostat-scraper/actions/workflows/deploy-cloudrun.yaml)
[![build](https://github.com/data-for-good-concepts/eurostat-scraper/actions/workflows/dry-run-docker-build.yaml/badge.svg)](https://github.com/data-for-good-concepts/eurostat-scraper/actions/workflows/dry-run-docker-build.yaml)

<!-- badges: end -->

> *⚠️ Please note, that the documentation of this project is still
> **Work In Progress**. The description might be incomplete and is
> subject to constant change.*

The **Eurostat Scraper** is a working example on how to wrap a web
scraper in a web service and deploy it on the cloud using R. This
project aims to showcase different aspects when it comes to web scraping
in general, as well as different technologies when deploying your work
in production.

This example scrapes the *‘Asylum applicants by type of applicant,
citizenship, age and sex - monthly data’* from the [**Eurostat Data
Browser**](https://ec.europa.eu/eurostat/databrowser/view/MIGR_ASYAPPCTZM/default/table?lang=en).
The scraper can be triggered by a `POST` request to the API endpoint in
which the scraper is wrapped in, and it returns the scraped data as part
of the `response` object. Even though this example has been developed to
scrape one particular dataset, any other dataset that is available as
part of the **Eurostat Data Browser** can be scraped with little to no
adjustments.

Following tools and technologies will be highlighted or are heavily in
use as part of this example - `RSelenium`, `plumber`, `Docker`,
`Google Cloud Build`, `Google Cloud Run` and `GitHub Actions`.

### Usage

In case you are running the scraping service locally, make sure you
start the web service by executing following command in your R console.

``` r
source('src/server.R')
```

#### Trigger scraping job

Start a scraping job by making a `POST` request to the trigger endpoint.
When you are working locally, replace `<URL>` with `localhost:8080`.
When you have the web service deployed on Google Cloud Run, use your
Cloud Run service URL.

``` bash
curl --location --request POST '<URL>/api/v1/scraper/job'
```

As a response the scraping service will return the requested dataset as
`JSON`.

``` js
[
    {
        "DATAFLOW": "ESTAT:MIGR_ASYAPPCTZM(1.0)",
        "LAST UPDATE": "11/11/22 23:00:00",
        "freq": "M",
        "unit": "PER",
        "citizen": "UA",
        "sex": true,
        "age": "Y14-17",
        "asyl_app": "ASY_APP",
        "geo": "AT",
        "TIME_PERIOD": "2021-12",
        "OBS_VALUE": 0
    },
    {
        "DATAFLOW": "ESTAT:MIGR_ASYAPPCTZM(1.0)",
        "LAST UPDATE": "11/11/22 23:00:00",
        "freq": "M",
        "unit": "PER",
        "citizen": "UA",
        "sex": true,
        "age": "Y14-17",
        "asyl_app": "ASY_APP",
        "geo": "AT",
        "TIME_PERIOD": "2022-01",
        "OBS_VALUE": 0
    },
    ...
]
```

### Deployment

In order to deploy this web service on Google Cloud, please make sure
you have a [Google Cloud Account](https://cloud.google.com/). Create a
project within your account and enable the
[`Cloud Build API`](https://console.cloud.google.com/apis/library/cloudbuild.googleapis.com)
as well as the
[`Cloud Run API`](https://console.cloud.google.com/apis/library/run.googleapis.com).
In order to deploy this service from the command line, you will further
need to install the
[`gcloud CLI`](https://cloud.google.com/sdk/docs/install).

> *⚠️ Documentation on IAM permissions need to be added. Further
> documentation on Service Account usage and permissions is currently
> missing.*

You can deploy this service on Google Cloud by executing the following
command in the command line. Make sure to replace `<REGION>` with the
[Google Cloud
region](https://cloud.google.com/compute/docs/regions-zones), you want
to use for the deployment of your web service.

``` bash
gcloud builds submit --region='<REGION>'
```

In case you want to modify deployment parameters, feel free to adjust
the `cloudbuild.yaml` according to your needs.

------------------------------------------------------------------------

Please note that this project is released with a [Contributor Code of
Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree
to abide by its terms.
