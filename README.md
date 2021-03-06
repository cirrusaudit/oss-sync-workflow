# oss-sync-workflow
GitHub Workflow example to sync a directory with an Alibaba Cloud OSS bucket.

# Template
```yaml
name: Push to Alibaba Cloud OSS

on:
  push:
    branches: [ master ]

env:
  BUCKET: bucket_name_here
  ENDPOINT: oss_endpoint_here
  ACCESS_KEY: access_key_here
  ACCESS_KEY_SECRET: ${{ secrets.ACCESS_KEY_SECRET }}

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Install Alibaba Cloud OSSUTIL
        run: wget http://gosspublic.alicdn.com/ossutil/1.6.10/ossutil64 && chmod +x ossutil64
      - name: Configure Alibaba Cloud OSSUTIL
        run: ./ossutil64 config -i ${ACCESS_KEY} -k ${ACCESS_KEY_SECRET} -e ${ENDPOINT} -c .ossutilconfig
      - name: Uploads the web folder to the choosen OSS bucket
        run: ./ossutil64 --config-file .ossutilconfig cp ${{ github.workspace }}/web oss://${BUCKET} -r -f
```

# Usage
Clone this repo, fill the gaps in the `.github/workflows/oss-cp.yml` file and place the website under `web`.
