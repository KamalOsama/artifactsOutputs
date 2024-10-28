```
run: |
  file=$(find dist/assets/*.js -type f | head -n 1)
  echo "filename=$file" >> $GITHUB_OUTPUT
```
- `file=$(find dist/assets/*.js -type f | head -n 1)`
  - used to access the first file the has .js as extension

- `echo "filename=$file" >> $GITHUB_OUTPUT`
  - used to assign file value to filename output

- In this context, echo "filename=$file" >> $GITHUB_OUTPUT tells GitHub Actions that we want to create an output variable called filename with the value of file.

- `>>` means "append" to a file or output.

- $GITHUB_OUTPUT is a special file GitHub Actions uses to store outputs. When we use `>> $GITHUB_OUTPUT`, we’re adding a line to this file.