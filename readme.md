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

## Testing result

| MSHA and WEBSITE_ENABLE_BROTLI_ENCODING | Content-Encoding | Content-Length |
| -------- | -------- | -------- |
| Win, true | br | 52 |
| Win, false | gzip | 1214 |
| Win, - | gzip | 1214 |
| Linux Container, true | - | 100042 |
| Linux Container, false | - | 100042 |
| Linux Container, - | - | 100042 |
