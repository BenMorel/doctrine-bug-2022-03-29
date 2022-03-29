# Doctrine bug

This bug happens when string columns have a `collation` set.

## How to reproduce

- Set `DATABASE_URL` in `.env` or `.env.local`
- Run `bin/console doctrine:database:create`
- Run `bin/console doctrine:migrations:diff`
    ```
    Generated new migration class to "/home/benjamin/src/roketto/packages/backend/bug/migrations/Version20220329140701.php"
    
    To run just this migration for testing purposes, you can use migrations:execute --up 'DoctrineMigrations\\Version20220329140701'
    
    To revert the migration you can use migrations:execute --down 'DoctrineMigrations\\Version20220329140701'
- Execute the migration: `bin/console doctrine:migrations:migrate -n`
    ```
    [notice] Migrating up to DoctrineMigrations\Version20220329140701
    [notice] finished in 18.6ms, used 14M memory, 1 migrations executed, 1 sql queries
    ```
- Check the schema: `bin/console doctrine:schema:validate`
    ```
    Mapping
    -------
    
    [OK] The mapping files are correct.
    
    Database
    --------
    
    [ERROR] The database schema is not in sync with the current mapping file.                                              
    ```

At this point, running `doctrine:migrations:diff` again generates the same migration again and again:

```sql
ALTER TABLE user CHANGE email email VARCHAR(100) NOT NULL COLLATE `ascii_bin`
```
