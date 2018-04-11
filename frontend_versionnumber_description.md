# Updating version number into front end (PROCESS DESCRIPTION DRAFT)

Jenkins runs a script which polls the alpha and beta instances of omaolo services. A log is held of the latest oda-backend and oda-web-front SHA:s. If a change is detected, then Jenkins uses the hash to poll Github for the commit messages from a period of ten days prior to the commit date associated with the SHA. The new hash is stored into a log to keep track of changes made in the omaolo service. The front page of the omaolo service is updated to contain a text with the changes during the past ten days.

The logging system has the following directory and file structure:

```
environment -- alpha
                -- pilot 1
                -- pilot 2
            -- beta
                -- pilot 1
                -- pilot 2

```

where `environment`, `alpha` and `beta` are directories, and `pilot n` is a log file containing the latest SHAs for the front and backend.
