To use the .env file

```
export $(cat .env | grep -v ^# | xargs)
```

To generate a cer0.pem -- all into one

```
openssl req -x509 -newkey rsa:2048 -keyout cert0.pem -out cert0.pem -nodes -subj '/CN=*'
```

To generate a normal pair of certificates
```
openssl req -x509 -new -newkey rsa:2048 -nodes -out beer42.local.cert.pem -keyout beer42.local.key.pem -subj '/C=DE/ST=Bayern/L=Munich/O=Valentin Vieriu/OU=Web/CN=*.beer42.local'
```

To protect endpoint with password, generate one using `htpasswd -nb admin secure_password` and then add the label `- traefik.frontend.auth.basic=admin:$$apr1$$QHDw9opb$$44ytr7T9LOkkrgocx4Y4o1'` by replacing any `$` with `$$`
If you are using .env for password, `$$` is not necesarry.


# Database Backup

```
-- Save the database using portainer console
pg_dump -U postgres -W -F t booksclub > /var/lib/postgresql/data/backup_booksclub_`date +"%d-%m-%Y"`.tar.gz
-- You will need to provide the password.
-- restore the database using portainer console
pg_restore -U postgres -W --dbname=booksclub --verbose /var/lib/postgresql/data/backup_booksclub_29-01-2017.tar.gz

-- to drop the old database use
dropdb -U postgres booksclub
createdb -U postgres booksclub
```
