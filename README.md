## Steps to replace file path, file name and file content
### Init --> define function and mapper
```python
def replace_all(src='', new_name='', existed_name='', file_expression=None):
    renames = {existed_name.lower(): new_name.lower(), existed_name.capitalize(): new_name.capitalize()}
```
### Rename directories and sub-directories
```python
    for root, sub_dirs, files in os.walk(src, topdown=False):
        for name in sub_dirs:
            for r in renames:
                if r in name:
                    new_dir_name = name.replace(r, renames[r], 1)

                    old_path_name = os.path.join(root, name)
                    new_path_name = os.path.join(root, new_dir_name)
                    os.rename(old_path_name, new_path_name)
```
### Rename file content
```python
    for root, sub_dirs, files in os.walk(src, topdown=False):
        for f in files:
            # change file content
            if file_expression is not None and re.match(file_expression, f):
                with open(os.path.join(root, f)) as file:
                    s = file.read()
                for r in renames:
                    s = s.replace(r, renames[r])
                with open(os.path.join(root, f), "w") as file:
                    file.write(s)
```
### Rename file name
```python
    # change file name
    for root, sub_dirs, files in os.walk(src, topdown=False):
        for f in files:
            for r in renames:
                if r in f:
                    newFile = f.replace(r, renames[r], 1)
                    try:
                        os.rename(os.path.join(root, f), os.path.join(root, newFile))
                        print("Renaming ", f, "to", newFile)
                    except FileNotFoundError as e:
                        print(str(e))
```
