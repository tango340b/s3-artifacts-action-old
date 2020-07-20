# Upload Artifacts to S3

The purpose of this action is to upload the zipped artifact from github actions and upload it to S3, segregated by repository and branch. 

This allows the organisation to package and upload all their artifacts to the same S3 bucket for deployment.

## Usage

This action requires 3 inputs: AWS Access Key Id, Secret Access Key, and the name of the zipped distribution file created by the github action.
You can give an optional  S3 Bucket Name, and  AWS Region. The defaults are that of our dev environment.
The following a snippet of how this can be used. You can also find the same example snippet is also available at ```.github/workflows/build.yml``` of this repository

```yaml

name: build

on: [push, pull_request]

jobs:
  publish-s3-artifact:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Upload to S3 as artifact
        uses: medlypharmacy/s3-artifacts-action@master
        with:
          aws_access_key_id: ${{ secrets.S3_BUILD_ARTIFACTS_ACCESS_KEY_ID}}
          aws_secret_access_key: ${{ secrets.S3_BUILD_ARTIFACTS_SECRET_ACCESS_KEY}}
          dist_file_path: './dummy91.zip'
```

## Optional Parameters

You can use additional optional parameters depending on your usecase. You can specify a custom S3 bucket. You can also specify whether the upload needs to go to a custom path. Also, if the source and destination is a directory, you can specify the resource type as a directory. If nothing is specified, it is assumed that the DIST file is a zip file.

```yaml

publish-s3-artifact-folder:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Upload to S3 artifact folder
        uses: medlypharmacy/s3-artifacts-action@master
        with:
          aws_access_key_id: ${{ secrets.S3_BUILD_ARTIFACTS_ACCESS_KEY_ID }}
          aws_secret_access_key: ${{ secrets.S3_BUILD_ARTIFACTS_SECRET_ACCESS_KEY }}
          aws_s3_bucket_name: ${{ secrets.S3_BUCKET_NAME }}
          dist_file_path: './dummyfolder'
          destination_path: "/dummyartifacts"
          resource_type: "DIRECTORY"     
```
