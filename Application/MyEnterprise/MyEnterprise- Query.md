---
tags:
  - query
  - MyEnterprise
---

## Report Weekly
### Referal Code
```sql
SELECT
o.id,o.name,o.customerId,o.channelContractNumber,o.paymentOption,o.paymentOption,o.category,o.created_at AS ocreatedat,o.status AS ostatus, c.id,c.name,c.npwpNumber,c.companySize,c.typeOfBusiness,c.created_at AS ccreatedat, ass.id AS asid,ass.email AS asemail,ass.position AS asposition,ass.salutation AS assalutation,ass.fullName AS asfullname,ass.firstName AS asfirstname, ass.lastName AS aslastname ,ass.created_at AS asscreated_at,
pic.id AS picid,pic.email AS picemail,pic.contactNumberIddCode AS piccontactNumberIddCode,pic.contactNumber AS piccontactnumber,pic.position AS picposition,pic.salutation AS picsallute,pic.fullName AS picfullname,pic.firstName AS picfirstname,pic.lastName AS piclastname,pic.created_at AS picreated,
ad.city,ad.province,ad.addressType,ad.postalCode,ad.created_at AS adcreated,o.referralCode FROM orders o
JOIN customer c ON c.id = o.customerId
JOIN as_user ass ON ass.id = c.asId
JOIN pic_user pic ON pic.id = c.picId
JOIN address ad ON ad.customerid = c.id
WHERE o.created_at >= '2024-12-23' AND o.created_at <= '2024-12-29' AND o.deleted_at IS  NULL AND ad.addressType = 'Office';
```

### User
```sql
SELECT pu.firstName, pu.lastName, pu.email, detail.corporate_pic_id, pu.created_at
FROM personal_users pu
JOIN personal_user_details detail ON pu.id = detail.personalUserId
WHERE pu.created_at >= '2024-12-23' AND pu.created_at <= '2024-12-29';
```

### Corporate 

```sql
SELECT o.id, o.`status` , dp.name,  dp.businessProductId, o.crmCustomerAccountId, crmRequestId ,c.name AS corporate_name, YEAR(o.created_at),COUNT(dp.price) AS qty, SUM(dp.price) AS price, o.created_at, o.updated_at FROM orders o 
JOIN digicore_products dp ON o.id = dp.orderId 
JOIN customer c ON c.id = o.customerId
WHERE o.created_at >= '2024-12-23' AND o.created_at <= '2024-12-29' AND o.`status` = 'Tsel Approved'
GROUP BY  dp.businessProductId, dp.name, YEAR(o.created_at), o.id, o.`status`;
```
### New Corporate

```sql
SELECT o.id, o.`status` , dp.name,  dp.businessProductId, o.crmCustomerAccountId, crmRequestId ,c.name AS corporate_name, YEAR(o.created_at),COUNT(dp.price) AS qty, SUM(dp.price) AS price, o.created_at FROM orders o
JOIN  digicore_products dp ON o.id = dp.orderId
JOIN customer c ON c.id = o.customerId
WHERE o.created_at >= '2024-12-23' AND o.created_at <= '2024-12-29' AND o.`status` = 'Tsel Approved' AND o.crmRequestId NOT LIKE 'A007%'
GROUP BY  dp.businessProductId, dp.name, YEAR(o.created_at), o.id, o.`status`;
```

### E+
```sql
SELECT o.id,YEAR(o.created_at),MONTH(o.created_at), o.`type`,o.subType,o.invoiceNumber, c.selectionPhoneNumber as msisdn, dp.price, dp.offerId, o.paymentMethod, o.lastStatus, o.lastCmStatus,o.paymentCallAt, o.created_at FROM b2c_orders o
JOIN b2c_carts c ON o.id= c.orderId
JOIN b2c_digicore_products dp ON c.productId = dp.id
WHERE o.created_at >= '2024-12-16' and o.created_at <= '2024-12-22' and o.lastCmStatus = 'Approved'
ORDER BY o.created_at desc  ;
```

### Quota Pooling
```sql
SELECT o.id,pay.invoice_number,pilot.msisdn,p.external_offer_id,
pay.total, pay.pay_date,pay.payment_method,pay.status AS
payment_status, o.status ,o.created_at
FROM orders.orders o
JOIN group_management."group" g ON g.id::TEXT = o.group_id
JOIN group_management.subscriber pilot ON pilot.id = g.pilot_id
JOIN package.packages p ON p.id::TEXT = o.package_id
LEFT JOIN payment.payment pay ON o.id::TEXT = pay.order_id
WHERE o.created_at >= '2024-12-01' AND o.created_at <= '2024-12-10'
AND pay.status = 'Success';
```
