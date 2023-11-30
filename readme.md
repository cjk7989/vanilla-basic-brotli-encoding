# How to Verify the Content Length

Run the following commands in cmd:

## Enable WEBSITE_ENABLE_BROTLI_ENCODING

curl -s -w "%{size_download}\n" -o nul -H "Accept-Encoding: br" https://agreeable-sand-04cc0e70a.jikun2static.net/

The result should be: 56

## Disable WEBSITE_ENABLE_BROTLI_ENCODING

curl -s -w "%{size_download}\n" -o nul -H "Accept-Encoding: gzip" https://agreeable-sand-04cc0e70a.jikun2static.net/

The result should be: 1214

## Without Compression

curl -s -w "%{size_download}\n" -o nul https://agreeable-sand-04cc0e70a.jikun2static.net/

The result should be: 100042