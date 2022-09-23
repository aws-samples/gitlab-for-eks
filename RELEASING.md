
1. Update the site version in config.toml
1. On the local machine, in the hugo root dir, run `hugo server` (Install hugo with brew)
2. In another console, run `rm -rf /tmp/pdfs; mkdir -p /tmp/pdfs;  npx website2pdf --output-dir /tmp/pdfs` (npm install website2pdf)
3. Use https://combinepdf.com to combine the pdfs in order from the appropriate content folders
4. Copy the results over the ones in /content/010_introduction/pdfs (use the same names)