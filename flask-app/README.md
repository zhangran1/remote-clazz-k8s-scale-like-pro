### Python Flask demo app

Build docker image
```shell
docker build -t flask-demo:0.1 .
```

Tag docker image

```shell
docker tag flask-demo:0.1 gcr.io/{your-project-id}/flask-demo:0.1
```

Push docker image to GCP container registry
```shell
docker push gcr.io/{your-project-id}/flask-demo:0.1
```
