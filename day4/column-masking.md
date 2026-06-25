```sql

-- CREATE DATABASE IF NOT EXISTS usersdb;


-- PII Personal Identification Information
CREATE OR REPLACE TABLE gksdb.users (
    name STRING,
    email STRING,
    phone STRING,
    aadhar STRING,
    pan STRING
);

INSERT INTO gksdb.users VALUES
  ('Gopal', 'gs@training.sh', '9876543210', '123456789012', 'ABCDE1234F'),
  ('Student', 'student2@gopalakrishnan.onmicrosoft.com', '8765432109', '987654321098', 'PQRSX5678Y');


SELECT current_user(); -- copy the output, put in into next function for admin


CREATE OR REPLACE FUNCTION gksdb.is_admin() RETURNS BOOLEAN
RETURN CURRENT_USER() = 'gopal@someone.onmicrosoft.com';

-- email is parameter passed from email column from users table
-- return value is masked value
CREATE OR REPLACE FUNCTION gksdb.mask_email(email STRING)
RETURNS STRING
RETURN 
  CASE 
    WHEN gksdb.is_admin() THEN email
    ELSE CONCAT(
      SUBSTR(email, 1, 2),
      '****',
      SUBSTR(email, LENGTH(email) - 5)
    )
  END;


-- testing
select gksdb.is_admin() ; -- true

-- apply the masking function to table columl, for every select statemetn, mask_email will be called, 
ALTER TABLE gksdb.users ALTER COLUMN email SET MASK gksdb.mask_email;

SELECT * FROM gksdb.users;

-- on second user student2@..



GRANT USE SCHEMA 
ON SCHEMA  gksdb
TO `srinath@someone.onmicrosoft.com`;

GRANT USE SCHEMA 
ON SCHEMA  gksdb
TO `srikanth@someone.onmicrosoft.com`;



GRANT USE SCHEMA 
ON SCHEMA  skdb
TO `gopal@someone.onmicrosoft.com`;

GRANT USE SCHEMA 
ON SCHEMA  skdb
TO `srinath@someone.onmicrosoft.com`;


GRANT SELECT ON TABLE gksdb.users TO `srinath@someone.onmicrosoft.com`;
GRANT SELECT ON TABLE gksdb.users TO `srikanth@someone.onmicrosoft.com`;
 
--srinath granting to gopal and srikanth,

GRANT SELECT ON TABLE srinathdb.users TO `srikanth@someone.onmicrosoft.com`;
GRANT SELECT ON TABLE srinathdb.users TO `gopal@someone.onmicrosoft.com`;


GRANT USE SCHEMA 
ON SCHEMA  skdb
TO `gopal@someone.onmicrosoft.com`;

GRANT USE SCHEMA 
ON SCHEMA  skdb
TO `srinath@someone.onmicrosoft.com`;


GRANT SELECT ON TABLE skdb.users TO `srinath@someone.onmicrosoft.com`;
GRANT SELECT ON TABLE skdb.users TO `gopal@someone.onmicrosoft.com`;



select * from srinathdb.users; 
select * from skdb.users; 


SELECT * FROM gksdb.users

CREATE OR REPLACE FUNCTION usersdb.mask_aadhar(aadhar STRING)
RETURNS STRING
RETURN 
  CASE 
    WHEN is_admin() THEN aadhar
    ELSE CONCAT('********', SUBSTR(aadhar, -4))
  END;
 

ALTER TABLE usersdb.users ALTER COLUMN aadhar SET MASK usersdb.mask_aadhar;

CREATE OR REPLACE FUNCTION usersdb.mask_pan(pan STRING)
RETURNS STRING
RETURN 
  CASE 
    WHEN is_admin() THEN pan
    ELSE '**********'
  END;

  ALTER TABLE usersdb.users ALTER COLUMN pan SET MASK usersdb.mask_pan;




```
