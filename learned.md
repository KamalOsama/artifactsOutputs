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

- Specifically, for GitHub Actions to recognize the output properly, you need to follow a particular format for echo when setting outputs. `you should set the output as file=<value> instead of directly echoing file`

```
  # Wrong
  file="example.txt"
  echo "$file" >> $GITHUB_OUTPUT
```

```
  # Right
  file="example.txt"
  echo "file=$file" >> $GITHUB_OUTPUT
```

---

# Caching

- Cache stores files for future runs of the same workflow, but it’s scoped to a specific job within the workflow.

- If you cache node_modules in the build job, that cache is available to the build job in future workflow runs, so the build job will use the cache each time it runs, speeding things up.

- However, if you have another job in the same workflow (like deploy) and it also needs node_modules, the deploy job won’t automatically use the cache created by the build job.

  - To use the same cached dependencies in deploy, you’d need to set up a separate cache action within the deploy job
  - or, alternatively, use artifacts to transfer the node_modules folder from build to deploy within the same workflow run.

- The `~/.npm` folder already contains cached files created by npm. When you install packages using npm install, npm automatically caches copies of the downloaded packages in this folder. This means:

  - npm Cache: Every time npm install runs, it checks the ~/.npm folder first to see if a package has already been downloaded and stored there.
  - Reusing Cached Packages: If a package is found in ~/.npm, npm uses it directly instead of downloading it again, making the install process faster.

- Caching executes two times each run, First, In it's turn (regular, Like all the steps), Runs another time after the total job finishes

---

# Environment variables & Secrets

- Environment variables are typically used to store non-secret data about database or API keys, (They can store anything, I am just saying people usually use it for that)

- secret secure data must be stored in enviornmnet secret

- When a workflow runs that builds the app, usually the app is connected to a database, The runner OS doesn't have access to our environment variables (.env.local), So we need to provide workflow enviornment variables with the same names and values, So that when the runner runs the workflow, It will not throw an undefined error -- (similar to the concept of mocking in testing)

- Environments
  - You can create the same secret key in multiple environments with a different value in each environment
  - Environments are created in github
  - In order to make a job run in certain environment you add `environmnet: environmnetName`
    - This will make that job have access to the values of the environment

  - mainly used if you have multiple databases (ex. one for testing, one for production, etc..)
    - Each database would have it's own name and password
    - So I will make an environment for each one, Then assigning the new values to the keys
    - Ex. db_username = TT5 (testing), db_username = TT1 (production)