How to run single docker instance to debug:
```bash
docker run --rm -v $(pwd):/src -p 1313:1313 --name website floryn90/hugo:0.127.0-ext-debian server
```
If for some reason website generates public html pages with URL at localhost:
```bash
grep -lr localhost public | xargs sed -i 's|http://localhost:1313|https://hub.sanchpet.su|g'
```
