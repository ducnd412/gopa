
* Create pipeline

```
curl -XPOST "http://localhost:8001/pipeline/config/" -H 'Content-Type: application/json' -d'
{
  "name": "discuss.elastic.co",
  "start": {
    "joint": "init_task",
    "enabled": true
  },
  "process": [
    {
      "joint": "url_normalization",
      "enabled": true,
      "parameters": {
        "follow_sub_domain": "true"
      }
    },
    {
      "joint": "chrome_fetch_v2",
      "enabled": true,
      "parameters": {
        "command": "google-chrome"
      }
    },
    {
      "joint": "parse",
      "parameters": {
        "dispatch_links": true,
        "max_depth": 100,
        "max_breadth": 100,
        "save_images": false
      },
      "enabled": true
    },
    {
      "joint": "html2text",
      "parameters": {},
      "enabled": true
    },
    {
      "joint": "content_deduplication",
      "parameters": {},
      "enabled": true
    },
    {
      "joint": "update_check_time",
      "parameters": {
        "decelerate_steps": "24h,48h,72h,168h,360h,720h",
        "accelerate_steps": "24h,12h,6h,3h,1h30m,45m,30m,20m,10m"
      },
      "enabled": true
    },
    {
      "joint": "lang_detect",
      "parameters": {},
      "enabled": true
    },
    {
      "joint": "save_snapshot_db",
      "parameters": {
        "max_revision": 5,
        "compress_enabled": true,
        "bucket": "Snapshot"
      },
      "enabled": true
    },
    {
      "joint": "index",
      "parameters": {},
      "enabled": true
    }
  ],
  "end": {
    "joint": "save_task",
    "enabled": true
  }
}'
```


* Assign a pipeline to host

```
curl -XPOST "http://localhost:8001/host/pipeline_config/" -H 'Content-Type: application/json' -d'
{
  "host":"discuss.elastic.co",
  "url_pattern":".*",
  "sort_order":1,
  "runner":"fetch",
  "pipeline_id":"b9154g2ihf5ln4c9ie10"
 }'
```


