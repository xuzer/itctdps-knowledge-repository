
## Mobile
### Cek User
```sql
SELECT
	user.username, fullname, user.nik, user.app_role_id, site.site_id, site.site_name, 
    app_role.app_role_name, user.status, user.created_at, user.updated_at
FROM user
INNER JOIN app_role ON user.app_role_id = app_role.id
INNER JOIN site ON user.site_id = site.site_id
WHERE username IN ('24364870');
```

## FMC

### Cek Slot  Notifcation

```sql
select * from slot_notification where order_id in ();
```