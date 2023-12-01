# Vanilla Basic Static Web App For Brotli Encoding Test

## How to Verify the Content Length

Run the following commands in cmd:
`curl -s -w "%{size_download}\n" -o nul -H "Accept-Encoding: gzip, deflate, br" https://agreeable-sand-04cc0e70a.jikun2static.net/`

### Enable WEBSITE_ENABLE_BROTLI_ENCODING

`curl -s -w "%{size_download}\n" -o nul -H "Accept-Encoding: gzip, deflate, br" https://agreeable-sand-04cc0e70a.jikun2static.net/`

The result should be: 56 (or 1214 if WEBSITE_ENABLE_BROTLI_ENCODING doesn't work)

### Disable WEBSITE_ENABLE_BROTLI_ENCODING

`curl -s -w "%{size_download}\n" -o nul -H "Accept-Encoding: gzip" https://agreeable-sand-04cc0e70a.jikun2static.net/`

The result should be: 1214

### Without Compression

`curl -s -w "%{size_download}\n" -o nul https://agreeable-sand-04cc0e70a.jikun2static.net/`

The result should be: 100042

### Testing result

| MSHA and WEBSITE_ENABLE_BROTLI_ENCODING | Content-Encoding | Content-Length |
| -------- | -------- | -------- |
| Win, true | br | 52 |
| Win, false | gzip | 1214 |
| Win, - | gzip | 1214 |
| Linux Container, true | - | 100042 |
| Linux Container, false | - | 100042 |
| Linux Container, - | - | 100042 |

## Update: Add CompressionProvider in Source Code

Added the following two CompressionProviders as mentioned in https://learn.microsoft.com/en-us/aspnet/core/performance/response-compression?view=aspnetcore-6.0
```
services.AddResponseCompression(options =>
{
    options.EnableForHttps = true;
    options.Providers.Add<BrotliCompressionProvider>();
    options.Providers.Add<GzipCompressionProvider>();
});
```

Note: For MSHA in Linux container, WEBSITE_ENABLE_BROTLI_ENCODING is not used.

### How to Verify the Content Length and Response Time

`curl -s -w "%{size_download}\n%{time_total}\n" -o nul -H "Accept-Encoding: gzip, deflate, br" https://agreeable-sand-04cc0e70a.jikun2static.net/` for brotli,

or `curl -s -w "%{size_download}\n%{time_total}\n" -o nul -H "Accept-Encoding: gzip, deflate" https://agreeable-sand-04cc0e70a.jikun2static.net/` for gzip,

or `curl -s -w "%{size_download}\n%{time_total}\n" -o nul https://agreeable-sand-04cc0e70a.jikun2static.net/` for original content.

Change the size of the content to 10 MB.

### Testing result

| MSHA and Encoding-Type | Content-Length (B) | Total-Time (s) |
| -------- | -------- | -------- |
| Win, br | 74 | 1.66 |
| Win, gzip | 105447 | 2.04 |
| Win, - | 10,000,026 | 4.37 |
| Linux Container, br | 3489 | 1.48 |
| Linux Container, gzip | 63157 | 2.20 |
| Linux Container, - | 10,000,026 | 5.29 | 
