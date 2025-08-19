#### Ambil crontab .sh file yang enabled
```sh
crontab -l  | grep -vE '^\s*(#|$)' | grep -oE '/[^ ]+\.sh'
```