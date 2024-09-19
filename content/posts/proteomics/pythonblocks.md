+++
title = "commonly used python codes"
bookToc=true
+++


### unzip compressed files into a new directory
```python
gz_file = os.path.join(root, file)
out_file = "new_path/" + file
with gzip.open(gz_file,"rb") as f_in, open(out_file,"wb") as f_out:
    shutil.copyfileobj(f_in, f_out)
```

