---
layout: post
title: Setting CORS for a Hetzner Bucket
description: "Ain't an obvious thing to do"
---

So, I needed to configure an S3-like bucket on Hetzner to work nicely with my Ruby on Rails 8 app that uses ActiveStorage to directly upload files to it.

As of mid-August 2025, Hetzner have no way to set CORS for their buckets via the web UI, so you'll have to do it manually, using CLI tools.

They have [docs](https://docs.hetzner.com/storage/object-storage/howto-protect-objects/cors/), yes, but they are not that helpful, and it'd be more useful for myself to have it documented.

So, here we go:

1. Install `s3cmd`
2. Configure your `s3cmd` with your Hetzner credentials
3. Create an XML file with the following content:


```xml
<CORSConfiguration>
  <CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
    <MaxAgeSeconds>3600</MaxAgeSeconds>
    <ExposeHeaders>ETag</ExposeHeaders>
    <ExposeHeaders>Content-Type</ExposeHeaders>
    <ExposeHeaders>Content-MD5</ExposeHeaders>
    <ExposeHeaders>Content-Disposition</ExposeHeaders>
  </CORSRule>
</CORSConfiguration>
```

4. Don't forget to change `AllowedOrigin` to your domain.
5. Run `s3cmd setcors your-cors-file.xml s3://your-bucket-name` to set CORS for your bucket.
6. Profit.

This CORS rules are sufficient for ActiveStorage to upload and download files, according to the [Rails docs](https://guides.rubyonrails.org/active_storage_overview.html#cross-origin-resource-sharing-cors-configuration).

Hetzner doesn't stop surprising me with their clunkiness.
